```cadence

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
import Stuff from 0x04
transaction() {
  prepare(signer: AuthAccount) {
    let nbaResource = signer.borrow<&Basketball.Test>(from: /storage/MyNBAResource)
                          ?? panic
    log(nbaResource.name)
  }

  execute {

  }
}


# Day 2

1. .link() allows you to link a resource to eithr a /public/ or /private/ path. 
2. Resource interfaces allows you to access certain pieces of data and can restrict others. 
3. 

pub contract Basketball {

  pub resource interface INBA {
    pub var name: String
  }
  pub resource Test: ITest {
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


i.
import Basketball from 0x03
    transaction() {
  prepare(signer: AuthAccount) {
    signer.save(<- Stuff.createNBA(), to: /storage/MyNBAResource)
    signer.link<&Basketball.NBA{Basketball.INBA}>(/public/MyTestResource, target: /storage/MyTestResource)
  }
  execute {
  }
}


ii.
import Stuff from 0x03
transaction(address: Address) {
  prepare(signer: AuthAccount) {
  }
  execute {
    let publicCapability: Capability<&Basketball.NBA> =
      getAccount(address).getCapability<&Basketball.NBA>(/public/MyTestResource)

    // ERROR: "The capability doesn't exist or you did not 
    // specify the right type when you got the capability."
    let nbaResource: &Basketball.Test = publicCapability.borrow() ?? panic
    nbaResource.changeName(newName: "Kobe")
  }
}

iii.

import Basketball from 0x03
transaction() {
  prepare(signer: AuthAccount) {
    signer.link<&Basketball.NBA>(/public/MyNBAResource, target: /storage/MyNBAResource)
  }
  execute {
  }
}

# Day 3 

# Day 4



```
