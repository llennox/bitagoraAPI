# bitagoraAPI
this is the api for bitagora.co

# so this is all just the api requests that the client side sends, I'll document it better in the future 

# BASE_URL = 'https://bitagora.co/'
# create account 

```javascript 
export const sendCreateAccount = (theemail, thepassword) => {
   return (dispatch) => {
     axios.defaults.withCredentials = false;
     const url = `${BASE_URL}api/create-user/`;
     axios.post(url, {
       email: theemail,
       password: thepassword
     }).then(function (response) {
       console.log(response.data);
       localStorage.setItem('token', response.data.token);
       localStorage.setItem('isAuthed', true);
       dispatch({type: MESSAGE, payload:'success! email sent'});
     }).catch(function (error) {
       console.log(error);
     })
   }
}
```

# log in and recieve an authtoken

```javascript 
export const loginUser = (theemail, thepassword) => {
   return (dispatch) => {
     axios.defaults.withCredentials = false;
     const url = `${BASE_URL}api/login/`;
     axios.post(url, {
       email: theemail,
       password: thepassword
     }).then(function (response) {
       localStorage.setItem('token', response.data.token);
       localStorage.setItem('isAuthed', true);
       dispatch({type: LOGIN_MESSAGE, payload: 'you are now logged in'})
     }).catch(function (error) {
       dispatch({type: LOGIN_MESSAGE, payload: 'there was a problem with your email or password'})
     })
   }
}
```

# this creates and order and address 

```javascript 
export const sendOrderNewAddress = (
  nameValue,
  apartmentValue,
  addressValue,
  countryValue,
  zipValue,
  additionalValue,
  phoneValue,
  thePrice,
  uuid,
  theUrl,
  theScreenshotUUID,
  quantityValue) => {

  return (dispatch) => {
    const token = localStorage.getItem('token');
    const localScreenshotUUID = localStorage.getItem('screenshot_uuid')
    axios.defaults.headers.common.Authorization = `Token ${token}`;
    const url = `${BASE_URL}api/orders/`;
    axios.post(url, {
      name: nameValue,
      apartment: apartmentValue,
      address: addressValue,
      country: countryValue,
      zip: zipValue,
      additional: additionalValue,
      phone: phoneValue,
      addressUUID: uuid,
      price: thePrice,
      url: theUrl,
      screenshotUUID: localScreenshotUUID,
      creatingOrder: true,
      quantity: quantityValue
    }).then(function (response) {
      localStorage.setItem('orders', response.data);
      dispatch({type: UPDATE_ADDRESSES, payload: response.data})
      dispatch({type: CHANGE_MESSAGE, payload: 'success'})
    }).catch(function (error) {
      console.log(error);
      dispatch({type: CHANGE_MESSAGE, payload: 'something went wrong'})
    })
  }
}

```

# this gets all orders from oldest to newest status is either paid, unpaid, or completed

```javascript 
export const getOrders = (status) => {
  return (dispatch) => {
    //axios.defaults.withCredentials = false;
    const token = localStorage.getItem('token');
    axios.defaults.headers.common.Authorization = `Token ${token}`;
    const url = `${BASE_URL}api/get-orders/`;
    axios.post(url, {
      option: status
    }).then(function (response) {
      dispatch({type: UPDATE_ORDERS, payload: response.data});
    }).catch(function (error) {
      console.log(error);
    })
  }
}
```

# returns payment address 
```javascript 
export const getPaymentAddress = (theorderUUID) => {
  return (dispatch) => {
    //axios.defaults.withCredentials = false;
    const token = localStorage.getItem('token');
    axios.defaults.headers.common.Authorization = `Token ${token}`;
    const url = `${BASE_URL}api/get-address/`;
    axios.post(url, {
       orderUUID: theorderUUID
    }).then(function (response) {
      dispatch({type: UPDATE_ORDERS, payload: response.data});
    }).catch(function (error) {
      console.log(error);
      dispatch({type: CHANGE_MESSAGE, payload: 'something went wrong'});
    })
  }
}
```
