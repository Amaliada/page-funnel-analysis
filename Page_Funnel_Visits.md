```python
import pandas as pd
```

Import all the files


```python
visits = pd.read_csv('visits.csv',
                     parse_dates=[1])
cart = pd.read_csv('cart.csv',
                   parse_dates=[1])
                   
checkout = pd.read_csv('checkout.csv',
                       parse_dates=[1])
purchase = pd.read_csv('purchase.csv',
                       parse_dates=[1])
```

Step 1: Inspect the DataFrames using `print` and `head`


```python
print(visits.head(5))
print(cart.head(5))
print(checkout.head(5))
print(purchase.head(5))
```

                                    user_id          visit_time
    0  943647ef-3682-4750-a2e1-918ba6f16188 2017-04-07 15:14:00
    1  0c3a3dd0-fb64-4eac-bf84-ba069ce409f2 2017-01-26 14:24:00
    2  6e0b2d60-4027-4d9a-babd-0e7d40859fb1 2017-08-20 08:23:00
    3  6879527e-c5a6-4d14-b2da-50b85212b0ab 2017-11-04 18:15:00
    4  a84327ff-5daa-4ba1-b789-d5b4caf81e96 2017-02-27 11:25:00
                                    user_id           cart_time
    0  2be90e7c-9cca-44e0-bcc5-124b945ff168 2017-11-07 20:45:00
    1  4397f73f-1da3-4ab3-91af-762792e25973 2017-05-27 01:35:00
    2  a9db3d4b-0a0a-4398-a55a-ebb2c7adf663 2017-03-04 10:38:00
    3  b594862a-36c5-47d5-b818-6e9512b939b3 2017-09-27 08:22:00
    4  a68a16e2-94f0-4ce8-8ce3-784af0bbb974 2017-07-26 15:48:00
                                    user_id       checkout_time
    0  d33bdc47-4afa-45bc-b4e4-dbe948e34c0d 2017-06-25 09:29:00
    1  4ac186f0-9954-4fea-8a27-c081e428e34e 2017-04-07 20:11:00
    2  3c9c78a7-124a-4b77-8d2e-e1926e011e7d 2017-07-13 11:38:00
    3  89fe330a-8966-4756-8f7c-3bdbcd47279a 2017-04-20 16:15:00
    4  3ccdaf69-2d30-40de-b083-51372881aedd 2017-01-08 20:52:00
                                    user_id       purchase_time
    0  4b44ace4-2721-47a0-b24b-15fbfa2abf85 2017-05-11 04:25:00
    1  02e684ae-a448-408f-a9ff-dcb4a5c99aac 2017-09-05 08:45:00
    2  4b4bc391-749e-4b90-ab8f-4f6e3c84d6dc 2017-11-20 20:49:00
    3  a5dbb25f-3c36-4103-9030-9f7c6241cd8d 2017-01-22 15:18:00
    4  46a3186d-7f5a-4ab9-87af-84d05bfd4867 2017-06-11 11:32:00
    

Step 2: Left merging visits and cart


```python
visits_cart = visits.merge(cart, how = 'left')

visits_cart.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>user_id</th>
      <th>visit_time</th>
      <th>cart_time</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>943647ef-3682-4750-a2e1-918ba6f16188</td>
      <td>2017-04-07 15:14:00</td>
      <td>NaT</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0c3a3dd0-fb64-4eac-bf84-ba069ce409f2</td>
      <td>2017-01-26 14:24:00</td>
      <td>2017-01-26 14:44:00</td>
    </tr>
    <tr>
      <th>2</th>
      <td>6e0b2d60-4027-4d9a-babd-0e7d40859fb1</td>
      <td>2017-08-20 08:23:00</td>
      <td>2017-08-20 08:31:00</td>
    </tr>
    <tr>
      <th>3</th>
      <td>6879527e-c5a6-4d14-b2da-50b85212b0ab</td>
      <td>2017-11-04 18:15:00</td>
      <td>NaT</td>
    </tr>
    <tr>
      <th>4</th>
      <td>a84327ff-5daa-4ba1-b789-d5b4caf81e96</td>
      <td>2017-02-27 11:25:00</td>
      <td>NaT</td>
    </tr>
  </tbody>
</table>
</div>




```python
Step 3: How long is `visits_cart`?
```


```python
total_visits = len(visits_cart)

print(total_visits)
```

    2000
    

Step 4: How many timestamps are null for `cart_time`?


```python
null_cart_times = len(visits_cart[visits_cart.cart_time.isnull()])

print(null_cart_times)
```

    1652
    

Step 5: What percentage only visited?


```python
visited_not_cart = float(null_cart_times) / float(total_visits)

print(visited_not_cart)
```

    0.826
    

Step 6: What percentage placed a t-shirt in their cart but did not checkout?


```python
cart_checkout = cart.merge(checkout, how = 'left')

print(cart_checkout.head())

null_checkout_times = len(cart_checkout[cart_checkout.checkout_time.isnull()])

cart_not_checkout = float(null_checkout_times) / float(len(cart))

print("Cart but not checkout:", cart_not_checkout)
```

                                    user_id           cart_time  \
    0  2be90e7c-9cca-44e0-bcc5-124b945ff168 2017-11-07 20:45:00   
    1  4397f73f-1da3-4ab3-91af-762792e25973 2017-05-27 01:35:00   
    2  a9db3d4b-0a0a-4398-a55a-ebb2c7adf663 2017-03-04 10:38:00   
    3  b594862a-36c5-47d5-b818-6e9512b939b3 2017-09-27 08:22:00   
    4  a68a16e2-94f0-4ce8-8ce3-784af0bbb974 2017-07-26 15:48:00   
    
            checkout_time  
    0 2017-11-07 21:14:00  
    1                 NaT  
    2 2017-03-04 11:04:00  
    3 2017-09-27 08:26:00  
    4                 NaT  
    Cart but not checkout: 0.3505747126436782
    

Step 7: Merge it all together


```python

all_data = visits_cart\
.merge(cart_checkout, how = 'left')\
.merge(purchase, how = 'left')

print(all_data.head(5))
```

                                    user_id          visit_time  \
    0  943647ef-3682-4750-a2e1-918ba6f16188 2017-04-07 15:14:00   
    1  0c3a3dd0-fb64-4eac-bf84-ba069ce409f2 2017-01-26 14:24:00   
    2  6e0b2d60-4027-4d9a-babd-0e7d40859fb1 2017-08-20 08:23:00   
    3  6879527e-c5a6-4d14-b2da-50b85212b0ab 2017-11-04 18:15:00   
    4  a84327ff-5daa-4ba1-b789-d5b4caf81e96 2017-02-27 11:25:00   
    
                cart_time       checkout_time       purchase_time  
    0                 NaT                 NaT                 NaT  
    1 2017-01-26 14:44:00 2017-01-26 14:54:00 2017-01-26 15:08:00  
    2 2017-08-20 08:31:00                 NaT                 NaT  
    3                 NaT                 NaT                 NaT  
    4                 NaT                 NaT                 NaT  
    

Step 8: % of users who got to checkout but did not purchase


```python
reached_checkout = all_data[~all_data.checkout_time.isnull()]

checkout_not_purchase = all_data[(all_data.purchase_time.isnull()) & (~all_data.checkout_time.isnull())]

checkout_not_purchase_percent = float(len(checkout_not_purchase)) / float(len(reached_checkout))

print("% of users who got to checkout but did not purchase:",checkout_not_purchase_percent)
```

    % of users who got to checkout but did not purchase: 0.24550898203592814
    

Step 9: check each part of the funnel, let's print all 3 of them again


```python
print("{} percent of users who visited the page did not add a t-shirt to their cart".format(round(visited_not_cart*100, 2)))
print("{} percent of users who added a t-shirt to their cart did not checkout".format(round(cart_not_checkout*100, 2)))
print("{} percent of users who made it to checkout  did not purchase a shirt".format(round( checkout_not_purchase_percent*100, 2)))
```

    82.6 percent of users who visited the page did not add a t-shirt to their cart
    35.06 percent of users who added a t-shirt to their cart did not checkout
    24.55 percent of users who made it to checkout  did not purchase a shirt
    

*The weakest part of the funnel is clearly getting a person who visited the site to add a tshirt to their cart. Once they've added a t-shirt to their cart it is fairly likely they end up purchasing it. A suggestion could be to make the add-to-cart button more prominent on the front page.*


Step 10: adding new column


```python
all_data['time_to_purchase'] = \
    all_data.purchase_time - \
    all_data.visit_time
```

Step 11: examine the results


```python
print(all_data.time_to_purchase)
```

    0                  NaT
    1      0 days 00:44:00
    2                  NaT
    3                  NaT
    4                  NaT
                 ...      
    2103               NaT
    2104               NaT
    2105               NaT
    2106               NaT
    2107               NaT
    Name: time_to_purchase, Length: 2108, dtype: timedelta64[ns]
    

Step 12: average time to purchase


```python
print(all_data.time_to_purchase.mean())
```

    0 days 00:43:12.380952380
    


```python

```
