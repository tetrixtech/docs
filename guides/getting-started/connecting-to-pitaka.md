# Connecting to Pitaka

### Web3 Browser Detection

To verify if the browser is running Pitaka, copy and paste the code snippet below in the developer console of your web browser:

```javascript
if (typeof window.ethereum !== 'undefined') {
  console.log('Pitaka is installed!');
}
```

### Connecting to Pitaka <a href="#connect-pitaka" id="connect-pitaka"></a>

"Connecting" or "logging in" to Pitaka effectively means "to access the user's account(s)".

You should only initiate a connection request in response to direct user action, such as clicking a button. You should always disable the "connect" button while the connection is pending. You should never initiate a connection request on page load.

We recommend that you provide a button to allow the user to connect Pitaka to your DApp. Clicking this button should call the following method:

```javascript
ethereum.request({ method: 'eth_requestAccounts' });
```

{% tabs %}
{% tab title="HTML" %}
```html
<button class="enablePitakaButton">Connect Wallet</button>
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const pitakaButton = document.querySelector('.enablePitakaButton');

pitakaButton.addEventListener('click', () => {
  //Will Start the Pitaka extension
  ethereum.request({ method: 'eth_requestAccounts' });
});
```
{% endtab %}
{% endtabs %}

This promise-returning function resolves with an array of hex-prefixed addresses, which can be used as a general account reference when sending transactions.

Since it returns a promise, if you're in an `async` function, you may log in like this:

```javascript
const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
const account = accounts[0];
```

{% tabs %}
{% tab title="HTML" %}
```html
<button class="enablePitakaButton">Connect Wallet</button>
<h2>Account: <span class="showAccount"></span></h2>
```
{% endtab %}

{% tab title="Javascript" %}
```javascript
const pitakaButton = document.querySelector('.enablePitakaButton');
const showAccount = document.querySelector('.showAccount');

pitakaButton.addEventListener('click', () => {
  getAccount();
});

async function getAccount() {
  const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
  const account = accounts[0];
  showAccount.innerHTML = account;
}
```
{% endtab %}
{% endtabs %}
