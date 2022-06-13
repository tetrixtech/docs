# Create a Simple DApp

{% hint style="info" %}
_**Pitaka is the same as Metamask's Setup and Installation.**_\
_****_\
_**Credits:**_ [_**https://docs.metamask.io/guide/create-dapp.html**_](https://docs.metamask.io/guide/create-dapp.html)_****_
{% endhint %}

### Project Setup <a href="#project-setup" id="project-setup"></a>

Before you set up make sure you've visited and gone through our [Getting Started Guide](<../README (1).md>)

Make sure you have:

1. The [Pitaka Extension](https://pitaka.io) downloaded.
2. Node.js [Downloaded and Installed(opens new window)](https://nodejs.org/)
3. Clone/Download the [Project Files (opens new window)](https://github.com/BboyAkers/simple-dapp-tutorial) from GitHub.
4. Your favorite Text Editor or IDE installed. I personally like [Visual Studio Code(opens new window)](https://code.visualstudio.com/)

#### Open Project Folder <a href="#open-project-folder" id="open-project-folder"></a>

Open the project folder. Navigate to `start`->`index.html`, and look at the comment stating part 1. This is the UI for the Simple Dapp with all of the layout and buttons that will help us learn the basics of connecting to our Pitaka extension. We will be using/building off of this entire section for the first part of the tutorial.

#### Install Dependencies <a href="#install-dependencies" id="install-dependencies"></a>

Open a terminal and make sure your terminal is inside the base directory of the `start/` folder. Inside the folder, the files should look like this:

```javascript
.
├─ index.html
├─ contract.js
├─ metamask.css
├─ package.json
└─ README.md
```

You'll have some more files but that's nothing to worry about!

Open your terminal and navigate into the start folder. In this folder run:

```
yarn install
```

This will install all the necessary dependencies we'll need for our project. This will have created a `node_modules/` folder where all the dependencies are stored.

Next run:

```markup
yarn run serve
```

Navigate to `http://localhost:9011`

### Basic Action(Part 1) <a href="#basic-action-part-1" id="basic-action-part-1"></a>

Now let's navigate into the contract.js file inside your start folder.

Your file should look something like this. Don't worry about lines 1-31.

```javascript
const forwarderOrigin = 'http://localhost:9010';

const initialize = () => {
  //You will start here
};
window.addEventListener('DOMContentLoaded', initialize);
```

As soon as the content in the DOM is loaded we are calling our initialize function. Now before we start writing any code let's look at our task list for the first part of this app.

What we'll cover in part one:

* [Connecting to Pitaka](../guides/getting-started/connecting-to-pitaka.md)
* See our eth\_accounts result
* Display our network number
* Display our ChainId
* Display our Accounts

#### Connecting to the Pitaka Wallet <a href="#connecting-to-the-metamask-wallet" id="connecting-to-the-metamask-wallet"></a>

The first thing we need to do in our Dapp is to connect to our Pitaka Wallet.

1. We need to create a function to see if the Pitaka Chrome extension is installed
2. If Pitaka is not installed we:
   1. Change our `connectButton` to `Click here to install` Pitaka
   2. When clicking that button it should take us to a page that will allow us to install the extension
   3. Disable the button
3. If Pitaka is installed we:
   1. Change our `connectButton` to `Connect`
   2. When clicking that button it should allow us to connect to our Pitaka wallet
   3. Disable the button

Let's get to it!!

#### Pitaka Extension Check <a href="#metamask-extension-check" id="metamask-extension-check"></a>

In our code we need to connect to our button from our index.html

```javascript
const initialize = () => {
  //Basic Actions Section
  const onboardButton = document.getElementById('connectButton');
};
```

Next we create a check function called `isPitakaInstalled` to see if the Pitaka extension is installed

```javascript
const initialize = () => {
  //Basic Actions Section
  const onboardButton = document.getElementById('connectButton');

  //Created check function to see if the Pitaka extension is installed
  const isPitakaInstalled = () => {
    //Have to check the ethereum binding on the window object to see if it's installed
    const { ethereum } = window;
    return Boolean(ethereum && ethereum.isPitaka);
  };
};
```

Next we need to create a `PitakaClientCheck` function to see if we need to change the button text based on if the Pitaka Extension is installed or not.

```javascript
const initialize = () => {
  //Basic Actions Section
  const onboardButton = document.getElementById('connectButton');

  //Created check function to see if the Pitaka extension is installed
  const isPitakaInstalled = () => {
    //Have to check the ethereum binding on the window object to see if it's installed
    const { ethereum } = window;
    return Boolean(ethereum && ethereum.isPitaka);
  };

  //------Inserted Code------\\
  const PitakaClientCheck = () => {
    //Now we check to see if Pitaka is installed
    if (!isPitakaInstalled()) {
      //If it isn't installed we ask the user to click to install it
      onboardButton.innerText = 'Click here to install Pitaka!';
    } else {
      //If it is installed we change our button text
      onboardButton.innerText = 'Connect';
    }
  };
  PitakaClientCheck();
  //------/Inserted Code------\\
};
```

#### Pitaka "Not Installed" Dapp Flow <a href="#metamask-not-installed-dapp-flow" id="metamask-not-installed-dapp-flow"></a>

In our code block where Pitaka isn't installed and we ask the user to `'Click here to install Pitaka!'`, we need to ensure that if our button is clicked we:

1. Redirect the user to the proper page to install the extension
2. Disable the button

```javascript
const PitakaClientCheck = () => {
  //Now we check to see if Pitaka is installed
  if (!isPitakaInstalled()) {
    //If it isn't installed we ask the user to click to install it
    onboardButton.innerText = 'Click here to install Pitaka!';
    //When the button is clicked we call this function
    onboardButton.onclick = onClickInstall;
    //The button is now disabled
    onboardButton.disabled = false;
  } else {
    //If it is installed we change our button text
    onboardButton.innerText = 'Connect';
  }
};
PitakaClientCheck();
```

We've created a function that will be called when we click the button and disable it. Let's dive into the `onClickInstall` function and create the logic inside of it.

Tip

For this part we will be using the '@metamask/onboarding' library we installed during the npm install. To learn more visit [here(opens new window)](https://github.com/MetaMask/metamask-onboarding#metamask-onboarding)

Inside this function we want to:

1. Change the text of the button to `Onboarding in progress`
2. Disable the button
3. Start the onboarding process

Above your `PitakaClientCheck` function write/insert this code.

```javascript
//We create a new Pitaka onboarding object to use in our app
const onboarding = new PitakaOnboarding({ forwarderOrigin });

//This will start the onboarding proccess
const onClickInstall = () => {
  onboardButton.innerText = 'Onboarding in progress';
  onboardButton.disabled = true;
  //On this object we have startOnboarding which will start the onboarding process for our end user
  onboarding.startOnboarding();
};
```

GREAT! Now if our end user doesn't have the Pitaka Extension they can install it. When they refresh the page the ethereum window object will be there and we can get on to connecting their Pitaka wallet to our Dapp!

#### Pitaka "Installed" Dapp Flow <a href="#metamask-installed-dapp-flow" id="metamask-installed-dapp-flow"></a>

Next we need to revisit our `PitakaClientCheck` function and create similar functionality that we did in our "Pitaka Not Installed" block in our "Pitaka Is Installed" block of code.

```javascript
const PitakaClientCheck = () => {
  //Now we check to see if Metmask is installed
  if (!isPitakaInstalled()) {
    //If it isn't installed we ask the user to click to install it
    onboardButton.innerText = 'Click here to install Pitaka!';
    //When the button is clicked we call th is function
    onboardButton.onclick = onClickInstall;
    //The button is now disabled
    onboardButton.disabled = true;
  } else {
    //If Pitaka is installed we ask the user to connect to their wallet
    onboardButton.innerText = 'Connect';
    //When the button is clicked we call this function to connect the users Pitaka Wallet
    onboardButton.onclick = onClickConnect;
    //The button is now disabled
    onboardButton.disabled = false;
  }
};
PitakaClientCheck();
```

Now we've created a function that will be called whenever we click the button to trigger a connection to our wallet, disabling the button. Next, let's dive into the `onClickConnect` function and build the logic we need inside of it.

Inside this function we want to:

1. Create an async function that will try to call the 'eth\_requestAccounts' RPC method
2. Catch any errors and log them to the console

Under your `onClickInstall` function write/insert this code.

```javascript
const onClickConnect = async () => {
  try {
    // Will open the Pitaka UI
    // You should disable this button while the request is pending!
    await ethereum.request({ method: 'eth_requestAccounts' });
  } catch (error) {
    console.error(error);
  }
};
```

Great! Now once you click the button the Pitaka Extension will pop up and connect your wallet.

#### Get Ethereum Accounts <a href="#get-ethereum-accounts" id="get-ethereum-accounts"></a>

After this what we'd like to do next is whenever we press the `eth_accounts` button we'd like to get our Ethereum account and display it. Replace the existing `const onboardButton` at the top of the `initialize()` function with the following three buttons:

```javascript
//Basic Actions Section
const onboardButton = document.getElementById('connectButton');
const getAccountsButton = document.getElementById('getAccounts');
const getAccountsResult = document.getElementById('getAccountsResult');
```

Now that we've grabbed our eth\_accounts button and our paragraph field to display it in we now have to grab the data.

Under our Pitaka`ClientCheck()` function let's write/insert the code below.

```javascript
//Eth_Accounts-getAccountsButton
getAccountsButton.addEventListener('click', async () => {
  //we use eth_accounts because it returns a list of addresses owned by us.
  const accounts = await ethereum.request({ method: 'eth_accounts' });
  //We take the first address in the array of addresses and display it
  getAccountsResult.innerHTML = accounts[0] || 'Not able to get accounts';
});
```

If you have already connected to your wallet in the previous section of the tutorial, you can go into Pitaka's "Connected Sites" menu and remove the localhost connection. This will enable us to test both buttons again. If we refresh our page, and click the "ETH\_ACCOUNTS" button, we should see `'eth_accounts result: Not able to get accounts'`.

Let's go ahead and press the "Connect" button again, and confirm the "Connect With Pitaka" prompt and "Connect". Now we can click the "ETH\_ACCOUNTS" button again and we should see our Pitaka account public address.

**CONGRATULATIONS!**

We have just completed building out our Basic Actions functionality. You know how to Connect to Pitaka, see your connected apps and remove them, as well as retrieve accounts.

Now on to our next step, showing our statuses.

Preview of the completed code up until this point of the tutorial:

```javascript
const forwarderOrigin = 'http://localhost:9010';

const initialize = () => {
  //Basic Actions Section
  const onboardButton = document.getElementById('connectButton');
  const getAccountsButton = document.getElementById('getAccounts');
  const getAccountsResult = document.getElementById('getAccountsResult');

  //Created check function to see if the Pitaka extension is installed
  const isPitakaInstalled = () => {
    //Have to check the ethereum binding on the window object to see if it's installed
    const { ethereum } = window;
    return Boolean(ethereum && ethereum.isPitaka);
  };

  //We create a new Pitaka onboarding object to use in our app
  const onboarding = new PitakaOnboarding({ forwarderOrigin });

  //This will start the onboarding proccess
  const onClickInstall = () => {
    onboardButton.innerText = 'Onboarding in progress';
    onboardButton.disabled = true;
    //On this object we have startOnboarding which will start the onboarding process for our end user
    onboarding.startOnboarding();
  };

  const onClickConnect = async () => {
    try {
      // Will open the Pitaka UI
      // You should disable this button while the request is pending!
      await ethereum.request({ method: 'eth_requestAccounts' });
    } catch (error) {
      console.error(error);
    }
  };

  const PitakaClientCheck = () => {
    //Now we check to see if Metmask is installed
    if (!isPitakaInstalled()) {
      //If it isn't installed we ask the user to click to install it
      onboardButton.innerText = 'Click here to install Pitaka!';
      //When the button is clicked we call th is function
      onboardButton.onclick = onClickInstall;
      //The button is now disabled
      onboardButton.disabled = false;
    } else {
      //If Pitaka is installed we ask the user to connect to their wallet
      onboardButton.innerText = 'Connect';
      //When the button is clicked we call this function to connect the users Pitaka Wallet
      onboardButton.onclick = onClickConnect;
      //The button is now disabled
      onboardButton.disabled = false;
    }
  };

  //Eth_Accounts-getAccountsButton
  getAccountsButton.addEventListener('click', async () => {
    //we use eth_accounts because it returns a list of addresses owned by us.
    const accounts = await ethereum.request({ method: 'eth_accounts' });
    //We take the first address in the array of addresses and display it
    getAccountsResult.innerHTML = accounts[0] || 'Not able to get accounts';
  });

  PitakaClientCheck();
};

window.addEventListener('DOMContentLoaded', initialize);
```
