Software Testing is the process of validating if the software meets its requirements. These requirements could be bug-free exectution, performance, compaitibility, API and finance.
## Types of Software Testing
![[/Untitled 7.png|Untitled 7.png]]
1. Manual Testing
    1. White Box Testing
    2. Black Box Testing
        1. Functional Testing
            1. Unit Testing
            2. Integration Testing
            3. System Testing
            4. User Acceptance Testing
        2. Non Functional Testing
            1. Compaitibility Testing
            2. Performance Testing
            3. Usability Testing
    3. Grey Box Testing
2. Automation Testing
### Manual Testing
The process of checking the functionality of an application as per the customer needs without taking any help of automation tools is known as manual testing.
The testing team has to write tests manually.
This is generally done using a framework, like, Catch2, JUnit, GoogleTest.
1. White Box Testing
2. Black Box Testing
3. Grey Box Testing
    
    ### White Box Testing
    
    The white box testing is done by Developer, where they check every line of a code before giving it to the Test Engineer. Since the code is visible for the Developer during the testing, that's why it is also known as White box testing.
    
    ### Black Box Testing
    
    The black box testing is done by the Test Engineer, where they can check the functionality of an application or the software according to the customer /client's needs. In this, the code is not visible while performing the testing; that's why it is known as black-box testing.
    
    ### Functional Testing
    
    The test engineer will check all the components systematically against requirement specifications is known as **functional testing**. Functional testing is also known as **Component testing**.
    
    ### Unit Testing
    
    Unit testing is the first level of functional testing in order to test any software. In this, the test engineer will test the module of an application independently or test all the module functionality is called **unit testing**.
    
    ### Integration Testing
    
    Once we are successfully implementing the unit testing, we will go integration testing. It is the second level of functional testing, where we test the data flow between dependent modules or interface between two features is called **integration testing**.
    
    ### Incremental Testing
    
    Suppose, we take two modules and analysis the data flow between them if they are working fine or not.
    
    If these modules are working fine, then we can add one more module and test again. And we can continue with the same process to get better results.
    
    In other words, we can say that incrementally adding up the modules and test the data flow between the modules is known as **Incremental integration testing**.
    
    1. Top-down Incremental Integration Testing
        
        In this approach, we will add the modules step by step or incrementally and test the data flow between them. We have to ensure that the modules we are adding are the **child of the earlier ones**.
        
    2. Bottom-up Incremental Integration Testing
        
        In the bottom-up approach, we will add the modules incrementally and check the data flow between modules. And also, ensure that the module we are adding is the **parent of the earlier ones**.
        
    
    ### Non Incremental Testing
    
    Whenever the data flow is complex and very difficult to classify a parent and a child, we will go for the non-incremental integration approach. The non-incremental method is also known as **the Big Bang method**.
    
    ### System Testing
    
    In system testing, the test environment is parallel to the production environment. It is also known as **end-to-end** testing.
    
    In this type of testing, we will undergo each attribute of the software and test if the end feature works according to the business requirement. And analysis the software product as a complete system.
    
    ### User Acceptance Testing
    
    Acceptance testing is done by the customers to check whether the delivered products perform the desired tasks or not, as stated in requirements.
    
    ### Alpha Testing
    
    This is a type of validation testing. It is a type of _acceptance testing_ which is done before the product is released to customers. It is typically done by QA people.
    
    ### Beta Testing
    
    The beta test is conducted at one or more customer sites by the end-user of the software. This version is released for a limited number of users for testing in a real-time environment.
    
    ### Non Functional Testing
    
    This testing technique doesn’t check the implementation of software, and tests it from user’s prespectives.
    
    ### Performance Testing
    
    We check the execution of the software.
    
    - **Load Testing**
    - **Stress Testing**
    - **Scalability Testing**
    - **Stability Testing**
    
    ### Usability Testing
    
    Another type of **non-functional testing** is **usability testing**. In usability testing, we will analyze the user-friendliness of an application and detect the bugs in the software's end-user interface.
    
    ### Compaitbility Testing
    
    In compatibility testing, we will check the functionality of an application in specific hardware and software environments. Once the application is functionally stable then only, we go for **compatibility testing**.
    
    ### Grey Box Testing
    
    Gray box testing is a combination of white box and Black box testing. It can be performed by a person who knew both coding and testing. And if the single person performs white box, as well as black-box testing for the application, is known as Gray box testing.
    
### Automation Testing
The tester writes scripts and uses another software to test the product. This process involves the automation of a manual process.
Example, Template testing and Git workflows.
  
### Regression Testing
Regression testing is a method of testing that is used to ensure that changes made to the software do not introduce new bugs or cause existing functionality to break. It is typically done after changes have been made to the code, such as bug fixes or new features, and is used to verify that the software still works as intended.
### Smoke Testing
This test is done to make sure that the software under testing is ready or stable for further testing
It is called a smoke test as the testing of an initial pass is done to check if it did not catch the fire or smoke in the initial switch on.
## Test Stubs and Drivers
|   |   |
|---|---|
|Stubs|Drivers|
|A section of code that imitates the called function is known as Stubs.|A section of code that imitates the calling function is known as Drivers.|
|It is used to test the functionality of modules and test modules and also replicate the performance of the lower-level module which are not yet merged, and the activity of the missing module/components.|When the main module is prepared or ready, we will take the help of drivers. Generally, drivers are a bit more complex as compared to the stubs.|
|The stubs are developed during the top-down incremental integration testing.|The drivers are developed during the bottom-up incremental integration testing.|
|Stubs replicate the activity of not developed and missing modules or components.|Drivers authorizes test cases to another system and which refer the modules under testing.|
|The stubs are created by the team of test engineers.|Mostly, the drivers are created by the developer and the unit Test engineers.|
|Stubs are developed when high-level modules are tested, and lower-level modules are not formed.|Drivers are acquired when lower-level modules are tested, and higher-level modules are not yet developed.|
|These are parallel to the modules of the software, which are under development process.|On the other hand, the drivers are used to reminding the component, which needs to be tested.|
|The stubs signify the low-level modules.|The drivers signify the high-level modules.|
|Fundamentally, the Stubs are also known as a called program and initially used in the Top-down integration testing.|The Drivers are also known as the calling program and are mainly used in bottom-up integration testing.|
|These are reserved for testing the feature and functionality of the modules.|The drivers are used if the core module of the software isn't established for testing.|
## Static Testing
Static testing is a verification process used to test the application without implementing the code of the application. It is a **cost-effective process**.
We can do some of the following important activities while performing static testing:
- Business requirement review
- Design review
- Code walkthroughs
- The test documentation review

> [!important]  
> Note: Static testing is performed in the white box testing phase, where the developer checks every line of the code before giving it to the Test Engineer.  
### Why we need Static Testing
- Dynamic Testing is time-consuming
- Flaws at earlier stages/identification of Bugs
- Dynamic Testing is expensive
- Increased size of the software