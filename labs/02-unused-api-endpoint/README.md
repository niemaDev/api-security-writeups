# Lab 2: Finding and Exploiting an Unused API Endpoint

- **Goal:** Exploit a hidden API endpoint to change the price of the **Lightweight "l33t" Leather Jacket** to $0.00, add it to your basket, and place the order.
- **Provided Credentials:** `wiener:peter`

---

## Setup 

When opening the lab inside Kali Linux, the built-in burp browser kept loading forever because of a compatibility issue with the host system's browser sandbox. 

* **The Fix:** Beginning burp suite from the terminal using the `--no-sandbox` flag fixed the hanging issue and allowed normal page loading and traffic interception.

![Launch Burp Suite by using terminal](./assets/sandbox.jpg)

To start working on the lab first open burp Suite's built-in browser and then log in to your PortSwigger account and browse the [PortSwigger API Testing Labs page](https://portswigger.net/web-security/api-testing).

---

## Step-by-Step Walkthrough

1. Go to Burp's browser and click the **Access the lab** button.   
   ![Access the lab](./assets/shop.jpg)
   and log in using the provided credentials: `wiener:peter`.
 ![Access the lab](./assets/login.jpg)
then Open Burp Suite, ensure Intercept is on, and browse to any product page (such as the Lightweight "l33t" Leather Jacket).
![Access the lab](./assets/interceptor.jpg)
2. Click on the product page.
   ![Product page](./assets/pro1.jpg)

3. In Burp, go to the **Proxy > HTTP history** tab and locate the `/api/products/1/price` request.
   ![HTTP history](./assets/http_history.jpg)

4. Right-click the API request and select **Send to Repeater**.
   ![Send to Repeater](./assets/get_price.jpg)

5. Go to the **Repeater** tab and change the HTTP method from `GET` to `OPTIONS`, then send the request. you can see the response specifies which methods are allowed.
   ![Options method response](./assets/option_price.jpg)

   change the HTTP method from `GET` to `POST`
   ![Post price check](./assets/post_price.jpg)

6. Change the method from `GET` to `PATCH`, then send the request. Notice that you receive an unauthorized message, indicating you need to be authenticated.
   ![Unauthorized patch response](./assets/patch_price.jpg)

7. when receive an Unauthorized response,  copy authentication details from your logged-in browser session into the Repeater request.
![Not allowed view](./assets/not_allowed.jpg)
    
 ![Not allowed view 2](./assets/not_allowed2.jpg)

8. Send the request. Notice that this causes an error due to an incorrect `Content-Type`

    ![Patch price payload](./assets/patch_price.jpg)


so we change it to `application/json`

 ![Error details](./assets/app_j.jpg)

9. Add an `{"price":"0"}` JSON object as the request body, then send the request. look at the error it mention that the body request has parameter error

 ![Content-Type error](./assets/app_json.jpg)

10.   Remove the `" "` on  price number `0`
 ![Supported content type](./assets/supported.jpg)

 ![Zero quote error](./assets/0qoute.jpg)
  

11. Before chaning the price api request the price of leather jacket was ``$1337.00`` 
    ![Jacket price zero](./assets/jacket.jpg)

12.Reload the leather jacket product page. See that the price of the leather jacket is now `$0.00`. Add the leather jacket to your basket, go to your basket, and click **Place order** to solve the lab.
    ![Lab solved](./assets/solve.jpg)

---
