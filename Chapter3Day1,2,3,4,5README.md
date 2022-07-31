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
    

# Day 2

1. <img width="1276" alt="array of birth" src="https://user-images.githubusercontent.com/105934102/182010507-4e7240ac-0bbe-422e-b9b4-caab28fb52a4.png">

<img width="1224" alt="arrayofdictionaries" src="https://user-images.githubusercontent.com/105934102/182010512-139fc37f-d1f3-4ed6-b60c-3e9cb95cf5a3.png">

# Day 3

1. <img width="1274" alt="dic" src="https://user-images.githubusercontent.com/105934102/182011413-b5ae8799-dfce-4ef4-bbce-3ecff434458e.png">
2. <img width="1272" alt="1" src="https://user-images.githubusercontent.com/105934102/182011780-bc7a4177-565c-49a1-91e4-1233ee7f5a4a.png">
3. References can be useful in Cadence because it doesn't involve moving resources. So you will be able to refer to a set of data without moving it.

# Day 4
1) Resource interfaces can be used as a requirement/condition for you to access data OR as a way to expose specific things. 

2) <img width="1271" alt="test" src="https://user-images.githubusercontent.com/105934102/182018469-9f0beae9-7fcb-445a-8c80-45261a6a10e4.png">



3) 

<img width="1278" alt="1" src="https://user-images.githubusercontent.com/105934102/182020096-b50aec56-83f4-4d1c-93d7-6811c2139fc5.png">
<img width="1278" alt="2" src="https://user-images.githubusercontent.com/105934102/182020097-052d9979-b6ce-41fc-9381-e245646a17bc.png">




# Day 5




a: read scope = 1,2,3,4; write scope = 1,2,3,4
b: read scope = 1,2,3,4; write scope = 1,2,3
c: read scope = 1,2,3 ; write scope = 1,2,3
d: read scope = 1,2,3 ; write scope = 1
