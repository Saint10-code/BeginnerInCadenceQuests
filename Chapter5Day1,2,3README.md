

# Day 1


1. An event is a way for us to notify everyone else that a change happened. 


2 & 3 

```cadence
pub contract BlooodPressureNFT {

pub event NFTMinted(id: UInt64)

pub resource NFT {
   pub let id: UInt64
   

   pub fun BloodPressure(x: Int, y: Int): Int {
    post {
      result > 130: "Your Blood Pressure Is High"
    }
    return x+y
   }
   
   init(){
    self.id = self.uuid
    
    emit NFTMinted(id: self.id)
    }

   }
}
```

4.  numberOne: Yes it will
    numberTwo: No it will not
    numberThree: No because you must specifiy the contract. self.number = 0
    
    
    
# Day 2

1. Standards can benefit FLOW because it'll help developers recognize different contracts easier. 
2. Rice with Chicken 

3. 



//The contract interface:

```cadence
  pub contract interface ITest {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    pre {
      newNumber >= 0: "We don't like negative numbers for some reason. We're mean."
    }
    post {
      self.number == newNumber: "Didn't update the number to be the new number."
    }
  }
  pub resource interface IStuff {
    pub var favouriteActivity: String
  }

  //We have to specifically implement the ITest.Istuff within the Stuff resource. 
  pub resource Stuff:IStuff {
    pub var favouriteActivity: String
  }
}
```

// The Implementing Contract:
```cadence
pub contract Test {
  pub var number: Int
  
  pub fun updateNumber(newNumber: Int) {
    self.number = newNumber
  }

//The contract interfacr specifies that we need to use ITest.Istuff.//

pub resource interface IStuff {
    pub var favouriteActivity: String
  }
  
  pub resource Stuff: ITest.IStuff {
    pub var favouriteActivity: String

    init() {
      self.favouriteActivity = "Playing League of Legends."
    }
  }

  init() {
    self.number = 0
  }
}
```


# Day 3

1. Force casting specifies the exact type of resource. This is beneficial for our collection because it ensures that only the sepcific type of NFT from a specific collection is deposited into our collection. 
2. The auth reference allows us to read more metadata of a specific resource. We use this whenever we want to read more metadata that is not available to us initially and whenever we want to downcast a type to read a specific type of resource.
3.

//Contract & resource interface//

``` cadence
import NonFungibleToken from 0x02

pub contract CryptoPoops: NonFungibleToken {
  pub var totalSupply: UInt64

  pub event ContractInitialized()
  pub event Withdraw(id: UInt64, from: Address?)
  pub event Deposit(id: UInt64, to: Address?)

  pub resource NFT: NonFungibleToken.INFT {
    pub let id: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid
      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
    }
  }

  pub resource interface ICryptoPoopsCollectionPublic{
    pub fun deposit(token: @NonFungibleToken.NFT)
    pub fun borrowAuthNFT(id: UInt64): &NFT
  }

  pub resource Collection: NonFungibleToken.Provider, NonFungibleToken.Receiver, NonFungibleToken.CollectionPublic, ICryptoPoopsCollectionPublic {
    pub var ownedNFTs: @{UInt64: NonFungibleToken.NFT}


    pub fun withdraw(withdrawID: UInt64): @NonFungibleToken.NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
            ?? panic("This NFT does not exist in this Collection.")
      emit Withdraw(id: nft.id, from: self.owner?.address)
      return <- nft
    }

    pub fun deposit(token: @NonFungibleToken.NFT) {
      let nft <- token as! @NFT
      emit Deposit(id: nft.id, to: self.owner?.address)
      self.ownedNFTs[nft.id] <-! nft
    }

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }

    pub fun borrowNFT(id: UInt64): &NonFungibleToken.NFT {
      return (&self.ownedNFTs[id] as &NonFungibleToken.NFT?)!
    }

      pub fun borrowAuthNFT(id: UInt64): &NFT {
      let ref = (&self.ownedNFTs[id] as auth &NonFungibleToken.NFT?)!
      return ref as! &NFT
    }
    init() {
      self.ownedNFTs <- {}
    }
    destroy() {
      destroy self.ownedNFTs
    }
  }
  pub fun createEmptyCollection(): @NonFungibleToken.Collection {
    return <- create Collection()
  }
  pub resource Minter {
    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }
    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }
  init() {
    self.totalSupply = 0
    emit ContractInitialized()
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
```


//Transaction to create a collection//

```cadence

import CryptoPoops from 0x01
import NonFungibleToken from 0x02

import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction() {
    prepare(signer: AuthAccount) {
        acoun.save(<- CryptoPoops.createEmptyCollection(), to: /storage/MyCryptoPoopCollection)
        acoun.link<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic,
                                            CryptoPoops.ICryptoPoopsCollectionPublic}>
            (/public/MyCryptoPoopCollection, target: /storage/MyCryptoPoopCollection)
    }
}
```

//Transaction to mint the nft//

```cadence
import CryptoPoops from 0x01
import NonFungibleToken from 0x02

transaction(receipent: Address, name: String, food: String, number: Int) {

  prepare(acct: AuthAccount) {
    let minter = account.borrow<&CryptoPoops.Minter>(from: /storage/Minter) ?? panic("No Crypto Poop to mint here.")
    let nft <- minter.createNFT(
            name: name,
            favouriteFood: favouriteFood,
            luckyNumber: luckyNumber )
            
   let recipientsCollection = getAccount(recipient)
            .getCapability(/public/MyCryptoPoopCollection)
            .borrow<&CryptoPoops.Collection{NonFungibleToken.CollectionPublic}>()
            ?? panic("No Crypto Poop here.")
        recipientsCollection.deposit(token: <- nft)
}
```

//Script to read metadata//

```cadence

import CryptoPoops from 0x01
import NonFungibleToken from 0x02

pub fun main(acct: Address, id: UInt64): String {
  let publicCryptoPoopCollection = getAccount(accoun).getCapability(/public/Collection).borrow<&CryptoPoops.Collection{CryptoPoops.ICryptoPoopsCollectionPublic}>() ?? panic("No Crypto Poop here.")

  return publicCryptoPoopCollection.borrowAuthNFT(id: id).luckyNumber
}
```
