# Code name Bad Bonny bebe


- [Threat Modeling](module_1/threat_modeling.md)

- [Threat Modeling Basics](module_1/threat_modeling_basics.md)

## Ilhas do Brasil

### Ilha de Santa Catarina

### Ilha das Aranhas

### Ilha de Florianópolis

Etapas do projeto:

- [3.1](#3.1) - Introduction To Broken Object Level Authorization
- [3.2](#3.2) - Introduction To Broken Function Level Authorization
- [3.2](#3.2) - Introduction To Broken Function Level Authorization
- [3.2](#3.2) - Introduction To Broken Function Level Authorization
- [3.2](#3.2) - Introduction To Broken Function Level Authorization


## 3.1

Introduction To Broken Object Level Authorization
Broken object level authorization is also known as Insecure Direct Object Reference (IDOR). This vulnerability allows an attacker to modify an object's ID value, which may result in unauthorized data access due to lack of authorization checks.

For example, a student should be the only user to access their grades. However, if the back-end API doesn't properly check for authorization, then other students may see the grades by inputting another student's ID (or increasing the ID parameter by one) and sending the API request again.

API requests often use an ID to reference an object in the request. This could appear in a URL as parameter, in the request header, or in a cookie. This ID maps to a resource on the back end such as file, data, or application. By changing this object ID in a request, an attacker may be able to gain access to or modify objects and data if there is a lack of authorization.

Reconnaissance
In the browser sandbox, there is a web application that is an online store that allows you log in and review your purchase history.

Log in to the application with the username alice and password monkey1. After logging in, you will get redirected to your order history page.

Click View Source and see if you can find the API request in JavaScript.

Now toggle the Intercept Requests switch to on, and click the Go button again. When the web proxy comes up, click Submit once until you see the GET request to the API.

Can you see a parameter that you may be able to change to get another user's data? If so, try changing it to see another user's data.

Weaponization and Attack
By analyzing the request, you can see that there is userid as an API parameter.

Make sure the web proxy has submitted all the pending requests. Now, click Go on the browser again. When you are in the intercept window for the API GET request, change the userid parameter value from 12045 to 12046 and click the Submit button. Are the results from another user?

Yes, the results are from the user bob, and you have found a broken object-level authorization vulnerability.

It appears that userid maps to the userid field in the database and loads orders from the user. How can you prevent this?

Defense Hint
The API endpoint shows a user's previous orders by ID, but does not verify if this order has been made by the current user. This allows the attacker to view another user's historical order data.

To safeguard against this, ensure that the user token belongs to the user ID in request by adding an authorization check before returning orders to the user.

Defending Against Broken Object Level Authorization
In this lesson, we learned that even with a valid token, an attacker may use broken implementation of authorization checks and get access to another customer's order history.

To protect against this, objects accessed via an API should always include authorization controls for every function, set up correct access permissions for users, and make sure all requests to resources require authorization validation.

Lastly, an attacker may try to guess or use brute force to find object IDs, so ensure IDs are random and unpredictable values (e.g., UUID).

Patch The Code
Now, open the Code Editor to look at the vulnerable code, you may select the language of your choice.

Add an authorization check so a user can only access their own order history. This can be done in the users query by checking the userid in addition to checking the apikey.

Once you have edited the code successfully, click the Save Code button. This will update the running application. Try exploiting the vulnerability again to make sure you have fixed the vulnerability.

```python
import os
import pymysql

def get_orders(userid, apikey):
    try:
        conn = pymysql.connect(host='db', port=3306, user='root', passwd='letmein', db='EcommerceApp')
        cursor = conn.cursor()

        # Check if the user with the provided userid and apikey exists
        cursor.execute("SELECT COUNT(*) FROM users WHERE apikey = %s and userid = %s;", (apikey, userid))
        rowcount = cursor.fetchone()[0]

        results = []
        if rowcount == 1:
            cursor = conn.cursor(pymysql.cursors.DictCursor)
            cursor.execute("SELECT ordername, orderid, userid, total FROM orders WHERE userid = %s;", (userid,))
            data = cursor.fetchall()

            result = {"results": data}
            results.append(result)

        conn.commit()
        cursor.close()
        conn.close()

        return results
    except Exception as e:
        print(e)
        raise e
```