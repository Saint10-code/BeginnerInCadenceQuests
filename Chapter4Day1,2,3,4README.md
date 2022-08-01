

# Day 1 

1.  Contract code - Multiple contracts can live inside the code. 
    Account Storage - container of data that lives within a certain path. (/storage/, /public/, /private/)
    
    
2.  /storage/ - Only the account owner can access the data inside here.
    /public/ - data that is available to everyone. 
    /private/ - only available to the account owner and the person that the account owner gives access to. 
    
    
    
3. .save() - allows you to save something to you account. <-
    .load() - allows you to take data out of your account storage. This will return an optional. <@> 
    .borrow() - used to get a reference to the resource in storage, not the resource itself. <&>

4. Scripts are only readable. Saving data involves moving the data, so you will not be able to do that in a script. 
  
5. You wouldn't be able to save your data to my account because my data lives in the /storage/ section of my account. The worst that could happen is that a hacker could accidentally sign a transaction that is in the prepare phase of my account. We probably shouldn't put sign requests in the prepare phase, but in the execute phase. 

6. 
```cadence
pub contract Basketball {

  pub resource NBA {
    pub var name: String
    init() {
      self.name = "Lebron"
    }
  }

  pub fun createNBA(): @NBA {
    return <- create NBA()
  }

}

 i. 
 
 import Basketball from 0x04
transaction() {
  prepare(signer: AuthAccount) {
    let nbaResource <- signer.load<@Basketball.NBA>(from: /storage/MyNBAResource)
                          ?? panic
    log(nbaResource.name) 

    destroy nbaResource
  }

  execute {

  }
}

ii. 
```cadence
import Basketball from 0x04
transaction() {
  prepare(signer: AuthAccount) {
    let nbaResource = signer.borrow<&Basketball.NBA>(from: /storage/MyNBAResource)
                          ?? panic
    log(nbaResource.name)
  }

  execute {

  }
}
```

# Day 2

1. .link() allows you to link a resource to eithr a /public/ or /private/ path. 
2. Resource interfaces allows you to access certain pieces of data and can restrict others. 
3. 

```cadence

pub contract Basketball {

  pub resource interface INBA {
    pub var name: String
  }
  pub resource NBA: INBA {
    pub var name: String

    pub fun changeName(newName: String) {
      self.name = newName
    }
    init() {
      self.name = "Lebron"
    }
  }
  pub fun createNBA(): @NBA {
    return <- create NBA()
  }
}
```

i.
```cadence

import Basketball from 0x03
    transaction() {
  prepare(signer: AuthAccount) {
    signer.save(<- NBA.createNBA(), to: /storage/MyNBAResource)
    signer.link<&Basketball.NBA{Basketball.INBA}>(/public/MyNBAResource, target: /storage/MyNBAResource)
  }
  execute {
  }
}
```


ii.
```cadence

import Basketball from 0x03
transaction(address: Address) {
  prepare(signer: AuthAccount) {
  }
  execute {
    let publicCapability: Capability<&Basketball.NBA> =
      getAccount(address).getCapability<&Basketball.NBA>(/public/MyNBAResource)

    // ERROR: "The capability doesn't exist or you did not 
    // specify the right type when you got the capability."
    let nbaResource: &Basketball.NBA = publicCapability.borrow() ?? panic
    nbaResource.changeName(newName: "Kobe")
  }
}
```

iii.
```cadence

import Basketball from 0x03
transaction() {
  prepare(signer: AuthAccount) {
    signer.link<&Basketball.NBA>(/public/MyNBAResource, target: /storage/MyNBAResource)
  }
  execute {
  }
}
```

# Day 3 
1. We added a collection to a contract so we minimize the number of storage paths and because no one can mint an NFT for us, we have to do it ourselves. 
2. You have to declare a "destroy" function that will destroy the nested resources. 
3. Maybe we can allow only the first 100 signers to mint an NFT or maybe we can allow the owner of an NFT of another collection to be able to mint an NFT.


# Day 4

```cadence

pub contract CryptoPoops {
  pub var totalSupply: UInt64

  // This is an NFT resource that contains a name,
  // favouriteFood, and luckyNumber
  pub resource NFT {
    pub let id: UInt64
  
  // This is the NFT resource ID: UInt64

    pub let name: String
    pub let favouriteFood: String
    pub let luckyNumber: Int
    
  // list of variables

    init(_name: String, _favouriteFood: String, _luckyNumber: Int) {
      self.id = self.uuid
      
  // initializing variables and linking the NFT resource ID to the UUID. UUID is unique. 
  
      self.name = _name
      self.favouriteFood = _favouriteFood
      self.luckyNumber = _luckyNumber
      
    }
  }

  // This is a resource interface that allows us to... you get the point.
  pub resource interface CollectionPublic {
    pub fun deposit(token: @NFT)
    pub fun getIDs(): [UInt64]
    pub fun borrowNFT(id: UInt64): &NFT
  }

  pub resource Collection: CollectionPublic {
    pub var ownedNFTs: @{UInt64: NFT}

// Creating a public collection of NFTs. 

    pub fun deposit(token: @NFT) {
      self.ownedNFTs[token.id] <-! token
    }
// allows NFT to be deposited in users account. 


    pub fun withdraw(withdrawID: UInt64): @NFT {
      let nft <- self.ownedNFTs.remove(key: withdrawID) 
              ?? panic("This NFT does not exist in this Collection.")
      return <- nft
    }
// allows NFT to be withdrawn from a users account. 

    pub fun getIDs(): [UInt64] {
      return self.ownedNFTs.keys
    }
    
// returns the users NFT keys. 

    pub fun borrowNFT(id: UInt64): &NFT {
      return (&self.ownedNFTs[id] as &NFT?)!
    }
// allows NFT to be borrowed from user

    init() {
      self.ownedNFTs <- {}
    }

    destroy() {
      destroy self.ownedNFTs
    }
  }

// NFT is initialized then destroyed

  pub fun createEmptyCollection(): @Collection {
    return <- create Collection()
  }
// a collection is being created 

  pub resource Minter {

    pub fun createNFT(name: String, favouriteFood: String, luckyNumber: Int): @NFT {
      return <- create NFT(_name: name, _favouriteFood: favouriteFood, _luckyNumber: luckyNumber)
    }
// NFT is created and variables of createNFT are returned with values. 

    pub fun createMinter(): @Minter {
      return <- create Minter()
    }

  }
  
// user is able to mint NFTs because of the Minter resource. 

  init() {
    self.totalSupply = 0
    self.account.save(<- create Minter(), to: /storage/Minter)
  }
}
// minter resource is saved to account storage.
```
