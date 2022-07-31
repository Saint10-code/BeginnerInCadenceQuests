# Chapter 3 Days 1,2,3,4 & 5


# Day 1 

1. Structs are just containers of data, Resources are more secure containers of data. Structs can be written anywhere while Resources can only be written within the contract. Resources use the @ symbol in front of their type so that we could know that we're dealing with a resource, while sturcts do not. 
2. A resource may be better than a struct when we want to input ultra secure information, for example medical records, social security number, etc. 
3. Create
4. No
5. Jacob
6.  pub fun createJacob(): @Jacob (missing the @ symbol)
    let myJacob <- create Jacob() (if you want to move a resource, you need the <- symbol and the word "create)
    return myJacob (doesn't have the <- symbol)
    
