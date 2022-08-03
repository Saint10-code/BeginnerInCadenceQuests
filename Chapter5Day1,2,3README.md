

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
    self.number = 5
  }

//The contract interfacr specifies that we need to use ITest.Istuff.//
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

//Contract//


