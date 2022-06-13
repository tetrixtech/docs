# Switch Chains based on DApp

```typescript
if (!window.ethereum) {
  window.ethereum = new Proxy(provider, {
    deleteProperty: () => true,
  });
  if (!window.web3) {
    window.web3 = {
      currentProvider: window.ethereum,
    };
  }
  window.ethereum.on('chainChanged', switchChainNotice);
}
window.dispatchEvent(new Event('ethereum#initialized'));
```
