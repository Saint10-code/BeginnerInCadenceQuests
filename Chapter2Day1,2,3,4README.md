# Chapter 2 Days 1,2,3,4


# Day 1


 <img width="1280" alt="contract" src="https://user-images.githubusercontent.com/105934102/181476871-9d18cbc4-59d5-4ff8-85e7-1b1966e9b021.png">
<img width="1280" alt="contract 1 " src="https://user-images.githubusercontent.com/105934102/181476899-1481be8a-f98d-47e6-aef0-509c3cf6e35b.png">


# Day 2

 1. Because scripts are Read-only. You can only read data in a script, you cannot change the data. 
 2. AuthAccount is a way to gain access to an account when a transaction is initialized during the prepare phase. 
 3. In the prepare phase is when data is being accessed in your account. In the execute phase, you cna call functions to change data and modify data.
 4. 
<img width="1273" alt="day 2 4" src="https://user-images.githubusercontent.com/105934102/181637619-4224760d-cc1a-41cd-81d3-b2499bd8e578.png">
<img width="1276" alt="mynew" src="https://user-images.githubusercontent.com/105934102/181642639-45017231-e4d3-4ef1-86a4-aee19811b083.png">
<img width="1280" alt="22222" src="https://user-images.githubusercontent.com/105934102/181642705-0158cd34-d45f-490b-b9e8-de473d895c4c.png">



# Day 3

1. <img width="1280" alt="favorite people" src="https://user-images.githubusercontent.com/105934102/182004658-d2334270-bdad-4957-8d0c-474d752f91a8.png">
2. <img width="1278" alt="socials" src="https://user-images.githubusercontent.com/105934102/182004663-9bb518bb-0c6f-4a15-a445-5648d3283168.png">
3. The force unwrap operator takes away an optional type and returns just the type. An optional type could either be, for example "A string, or nil". The forced unwrapped operater takes away this option of "nil" and just returns the "string". I will explain this using a forced unwrapped operator to log an integer. 
<img width="1272" alt="forced" src="https://user-images.githubusercontent.com/105934102/182004851-175ea1d0-2825-45e5-8b22-bee1798a0be6.png">
<img width="1270" alt="nil" src="https://user-images.githubusercontent.com/105934102/182004867-97bebaa9-6062-40f8-ad96-2b1027db2358.png">
4. So the function states that we should represent strings ONLY. However, the second line represents that we could either have a string OR an address. This would be an "optional", so this is why the error states "String?". It is because we are trying to put an optional type in a function that is only supposed to represent Strings only. We can fix this by removing the Addresses. 
