---
layout: post
title:  Art of error-handling in Salesforce Lightning Components 
---
![_config.yml]({{ site.baseurl }}/images/Content/error.jpg)

The journey of Lightning components to server-side controller involves many potential failure points that are often taken granted by us, the developers. In this blog, let's learn the art of error handling for Lightning components. Most of the Lightning applications rely on Apex to perform backend operations such as accessing and modifying data.
Here is the routine request-response pattern of a typical lightning application:
* Request the Apex controller using a server-side action.
* The controller processes the requests and all the regular Apex exceptions are applicable. 
* After the transaction, Apex controller sends a response to the Lightning component controller or helper which are on client-side.
* The Lightning  controller or helper processes the response in a callback function which sometimes can cause client-side errors (unexpected response).

In the above mentioned journey, there is a probability of getting trapped into server-side and client-side errors. Taking about the server-side controllers the below piece of code throws a null-pointer exception. 
```
    try {
		string s;
        s.toLowerCase(); // Causes Null Pointer exception 
    }
    catch (Exception e) {
        // "Convert" the exception into an AuraHandledException
        throw new AuraHandledException('Here is what's fishy: '
            + e.getMessage());    
    }
    
```
Now, let's see how we can handle the above error in a Lightning friendly way. 
Thankfully, the solution to this problem is quite simple.
1. Use Try-catch block to make sure the exceptions are caught.
2. Throw an AuraHandledException in the catch block. This allows you to provide a custom user-friendly error message.

```
// Best practice: Use AuraHandledException
@AuraEnabled
public static void triggerBasicAuraHandledError() {
    try {
		string s;
        s.toLowerCase(); // Causes Null Pointer exception 
    }
    catch (Exception e) {
        // "Convert" the exception into an AuraHandledException
        throw new AuraHandledException('Here is what's fishy: '
            + e.getMessage());    
    }
    finally {
        // Final actions like sending an email, field updates etc. 
    }
}



```

The AuraHandledException is sent back to the client with your custom message, and you’re free to display it in the way you want using the Lightning Component framework.As AuraHandledException cannot be extended, in-order to throw more complex exceptions there is a way to create a wrapper class and serialize  the data as shown below:

```
// Wrapper class for my custom exception data
public class CustomExceptionData {
    public String name;
    public String message;
    public Integer code;

    public CustomExceptionData(String name, String message, Integer code) {
        this.name = name;
        this.message = message;
        this.code = code;
    }
}

// Throw an AuraHandledException with custom data
CustomExceptionData data = new CustomExceptionData('MyCustomServerError', 'Some message about the error', 123);
throw new AuraHandledException(JSON.serialize(data));

// Finally, on the client-side (Lightning controller or helper), parse the error message string as JSON, and access your custom error data.

// Parse custom error data & report it
let errorData = JSON.parse(error.message);
console.error(errorData.name +" (code "+ errorData.code +"): "+ errorData.message);

```

Client-side errors on the other hand can be trivial which can be caused by a remote technical error such as an AuraHandledException or a value that does not meet certain business rules. Here is the Lightning function that can handle the apex response on client-side:

```
// Server-side action callback
function(response) {
    // Checking the server response state
    let state = response.getState();
    if (state === "SUCCESS") {
        // Process server success response
        let returnValue = response.getReturnValue();
    }
    else if (state === "ERROR") {
        // Process error returned by server
    }
    else {
        // Handle other reponse states
    }
}

```
If the state of the response is ERROR, you need to handle a server error. You can access the errors details with the response.getError() function which returns an array of errors, not just a single one. Below is the code to print all the errors to the console:

```
        let errors = response.getError();
        let message = 'Unknown error'; // Default error message
        // Retrieve the error message sent by the server
        if (errors && Array.isArray(errors) && errors.length > 0) {
            for (i = 0; i < errors.length; i++) { 
                message = message +'Error'+ i + ':' + errors[i].message;
            }
        }
        // Display the message
        console.error(message);


```

If the goal is to display the user an error message, then a toast message can be added as shown below:

```
let toastParams = {
        title: "Error",
        message: “Here is what you screwed up!“, // Default error message
        type: "error"
    };
    // Pass the error message if any
    if (errors && Array.isArray(errors) && errors.length > 0) {
               for (i = 0; i < errors.length; i++) { 
                message = message +'Error'+ i + ':' + errors[i].message;
            }
        toastParams.message = message;
    }
    // Fire error toast
    let toastEvent = $A.get("e.force:showToast");
    toastEvent.setParams(toastParams);
    toastEvent.fire();

```

It's healthy to use try-catch blocks on client-side controllers too as shown below:

```
try {
    // Something that could throw an error
    throw new Error('Error message goes here');
} catch (e) {
    // Error handling
    console.error(e);
} finally {
    // Something executed whether there was an error or not
}

```

Here is the Lightning component with state of the art error handling. 
![_config.yml]({{ site.baseurl }}/images/Content/Error.gif)

# Resources:
1. [Philippe Ozil](https://developer.salesforce.com/blogs/2017/09/error-handling-best-practices-lightning-apex.html)
2. [Dev Guide](https://developer.salesforce.com/docs/atlas.en-us.apexcode.meta/apexcode/apex_exception_definition.htm)
