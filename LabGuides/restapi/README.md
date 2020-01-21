# Exercise Creating REST testing script

[Go back to the Table of Contents](../../README.md)

For this synthetic test we are going to monitor the availability and response times of a REST API data source. 

## Accessing the Synthetic editor

### 1. Navigation to Synthetics editor

The synthetic editor is listed in the Administration tab at the top of the screen.  To navigate to the correct place, if not already in ICAM, first click the hamburger menu in the top left, select **“Monitor health”** then **"Infrastructure monitoring"** from the menu and then click on **“Administration”** in the resulting bar.

![](images/2020-01-20-09-09-57.png)

![](images/2020-01-20-07-42-06.png)

### 2. Selecting Synthetics

Once the screen loads you need to click on the area of interest. In this case **“Synthetics”**.

![](images/2020-01-20-07-44-08.png)

Click on the **Create** button 

![](images/2020-01-20-07-44-39.png)

### 3. Editing Title

We are again going to build another synthetic test, but this time using an API REST interface.
First give the new test a name. 
Use any name you wish. I am going to use the name `Pet Store – REST Availability` for this example.

![](images/2020-01-20-07-49-58.png)

### 4. Editing Description

Add a quick description of the monitoring you are setting up. This is optional, but it can be very useful for others that are reviewing the test configuration, or in the case of reviewing the test during an incident.

![](images/2020-01-20-07-50-15.png)

### 5. Selecting Type of Synthetic test

As we want to monitor a REST API we will select the **REST API** tile.

![](images/2020-01-20-07-50-45.png)

### 6. Selectimng Request Method

For the request method we need to select the drop-down and click on **POST** as we want to get data in the response to our test.

![](images/2020-01-20-08-54-56.png)

### 7. Providing Data source URL

You can pick any REST API you want in this exercise. I have provided an external URL for a test API interface which will allow you to run the test and see the data coming back.  In the URL box type:

`https://petstore.swagger.io/v2/store/order`

Note: If you want to see the data first before creating the test, enter the URL into a separate browser window and you can view its structure and information.

![](images/2020-01-20-08-56-41.png)

### 8. Editing test request Header and Header value

These are optional, but in this case we are doing a POST and we need to tell the API what type of data to expect. Add a header called `Content-Type` with value `application/json`.

![](images/2020-01-20-08-59-16.png)

### 9. Providing test Request Body

Here is where the details of the test order go. You can copy this block into the Request Body.
```
{
    "id": 0,
    "petId": 0,
    "quantity": 0,
    "shipDate": "2020-01-20T13:40:02.204+0000",
    "status": "placed",
    "complete": true
}
```
### 10. Response validation

Next, we are going to configure the warning and critical event thresholds for the test. 
NOTE: By default, a response time over 5 seconds will trigger a warning event and a response time over 10 seconds triggers a critical event. 

As this is a lab test feel free to leave or change the defaults. 

![](images/2020-01-20-07-52-27.png)

### 11. Authentication

Basic NTLM authentication (challenge-response authentication) is supported. This is also an optional field and we won’t need it for this test.

### 12. Verifying test

This button/step is used to verify the test configuration, Click the **Verify Test** button.

![](images/2020-01-20-07-53-59.png)

This will produce a pop-box and can take a minute or two depending on the complexity of the script ICAM is checking. When the validation completes you will be presented with a pass or fail icon.

Green = pass
Red = fail – Should any of your verifications fail you can click on the **Details** option to see which part of the script ICAM has failed.

In our case this test should pass and you will be presented with a green check mark.

![](images/2020-01-20-09-04-55.png)

Click **Next** in the bottom right corner.

![](images/2020-01-20-07-55-56.png)

### 13. Editing test settings

The next step is to set interval frequency of the synthetic test. In most production environments longer intervals are likely, but as this is a lab and we want to generate some data quickly, set the interval to 2 minutes.

With ICAM you can test from multiple locations around the globe. The **Testing Frequency** will allow you to either run the same synthetic test simultaneously from all the sites or stagger them. In this case leave **Simultaneous** selected.

![](images/2020-01-20-09-13-30.png)

### 14. Selecting Locations

This section will allow you to pick the sites from across the globe from which you wish to run the synthetic test from. You can pick a single site or multiple.

![](images/2020-01-20-07-58-25.png)

### 15. Editing Event Triggers

This is where you can set whether a failed response triggers a critical alert. A failed response is when the test returns a code 400 or above.  For this exercise we are going to set the slider bar to “Off”.

You can also set how many times the test threshold is breached before the event is sent. Again, this is going to be left at “0” for this exercise.

![](images/2020-01-20-07-58-57.png)

### 16. Completing the test creation

You have now completed the configuration of the REST API synthetic test. Click **Finish** in the bottom right corner of the screen

![](images/2020-01-20-07-59-48.png)

### 17. Exploring the Synthetic results

Now that you have created the synthetic test, it’s time to see how the API is performing. To do this you again will need to consult the synthetic results dashboard. Here is how to navigate to that page.

Click the **Synthetic Results** at the top of the screen

![](images/2020-01-20-08-01-20.png)

You will now see all the synthetic tests you have created. 

Select the script you just created by clicking on the blue name

![](images/2020-01-20-08-02-24.png)

As before you can see the timeline, time period features, locations, and widgets as we have looked at in previous exercises.  For this exercise, review the data in these widgets to be familiar with what is returned. Once you have finished this review, scroll down to the “Test instance breakdown” widget on the page.

This widget will give you a wealth of information about the synthetic test that you have performed. 

![](images/2020-01-20-08-02-51.png)

At the top you will see each playback result in time order.  This high-level view will show you everything from the response time of each playback to the response code.

NOTE: You can set up an incident to be raised if an unexpected return code is issued.

![](images/2020-01-20-08-03-23.png)

If you click the little arrow to the left of one of the playbacks, it will expand that test to show you the detailed data.

At the top you will see clear stat feedback on:
•	Response time
•	Redirect
•	Size
•	Download speed 
•	Errors seen

Then you have the stage breakdown of each of the stages of the test.  This is a great resource to see which part of the test the most time is spent should you be experiencing issues with your own environments. You can compare and contrast the results over time at a glance. This will allow you to see if this is a sudden change, or the response times have been growing slowly over time.

There is a visual representation of the test and also the time in milliseconds on the right-hand side of each of the stages.

![](images/2020-01-20-08-04-33.png)

As we have used an external source as the URL. You are more likely to see changes in the results.  Remember you can use the first (top) widgets on this screen to review data such as the average times and percentage availability.

[Go back to the Table of Contents](../../README.md)

<table>
  <tr>
    <td>Version</td>
    <td>1.0</td>
  </tr>
  <tr>
    <td>Author</td>
    <td>Sean Lombardo, IBM</td>
  </tr>
  <tr>
    <td>email</td>
    <td>sean.lombardo@ibm.com</td>
  </tr>
  <tr>
    <td>Author</td>
    <td>Cross Ganaway, IBM</td>
   </tr>
   <tr>
     <td>email</td>
     <td>cganawa@us.ibm.com</td>
   </tr> 
</table>