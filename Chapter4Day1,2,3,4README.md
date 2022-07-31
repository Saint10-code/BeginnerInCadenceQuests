```cadence

# Day 1 

1.  Contract code - Multiple contracts can live inside the code. 
    Account Storage - container of data that lives within a certain path. (/storage/, /public/, /private/)
    
    
2.  /storage/ - all of your data is here and only you can access this information. 
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


# Day 3 

# Day 4



```
