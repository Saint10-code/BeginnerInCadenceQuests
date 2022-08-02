

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



