# ๐จ Badgemeal Frontend

## โ start & build

### .env ์์ฑ

- `.env.development`
```
REACT_APP_NFT_CONTRACT_ADDRESS = "0x8Ec4f1881361fbcfD1CeA615C2AFa216668E2E2E"
REACT_APP_VOTE_CONTRACT_ADDRESS = "0x3166433C1FC37F52d0C6480ab3BD997dFEd23d5c"
REACT_APP_CHAIN_ID = "1001"
REACT_APP_ACCESS_KEY_ID = ""
REACT_APP_SECRET_ACCESS_KEY = ""
REACT_APP_DEPLOYER_PRIVATE_KEY = ""
```

- `.env.production`
```
REACT_APP_NFT_CONTRACT_ADDRESS = "0x5b35552c347301DDC6E5D0Cf5F1a4445E294Fb8c"
REACT_APP_VOTE_CONTRACT_ADDRESS = "0xA2d17c0C6E2102c57bC519D36b71F9c9BE2f59C3"
REACT_APP_CHAIN_ID = "8217"
REACT_APP_ACCESS_KEY_ID = ""
REACT_APP_SECRET_ACCESS_KEY = ""
REACT_APP_DEPLOYER_PRIVATE_KEY = ""
```

### webpack-dev-server๋ก ์คํ

`npm run start`

### Build

`npm run build`

### Lint

`npm run lint`

---

## โ ๊ฐ๋ฐํ๊ฒฝ

| ๋ถ๋ฅ | ๋ด์ฉ |
| --- | --- |
| ๊ธฐ์ ์คํ | JavaScript, SWR, React.js, caver-js, webpack, Materia UI |
| ์์กด์ฑ ๊ด๋ฆฌ ๋๊ตฌ | NPM |
| ์ฃผ์ ๊ฐ๋ฐ ๋๊ตฌ | Visual Studio Code, Chrome |

---

## โ ํ๋ก์ ํธ ๊ด๋ฆฌ

| ๋ถ๋ฅ  | ๋ด์ฉ |
| --- | --- |
| ๋ฐฐํฌ  | Nginx ์น์๋ฒ |
| ๋ฒ์  ๊ด๋ฆฌ ์์คํ | Git, GitHub |

---

## โ utils ํจํค์ง

- ๊ณต์ฉ์ผ๋ก ์ฌ์ฉํ๋ ํจ์๋ค๊ณผ ๊ด๋ จ๋ ํจํค์ง

### `fetcher.js`

- axios ์ธ์คํด์ค ์์ฑ
```js
export const Axios = axios.create({
  baseURL: API_BASE_URL,
  timeout: 10000,
});
```

- http GET ์์ฒญ
```js
export const getDataFetcher = async (url) => {
  const res = await Axios.get(url).catch(function (error) {
    if (error.response && error.response.status > 400) {
      // ์์ฒญ์ด ์ด๋ฃจ์ด์ก์ผ๋ฉฐ 400์ด์ ์๋ฌ๋ฅผ ์ฒ๋ฆฌ
      const requestError = new Error('An error occurred.');
      // ์๋ฌ ๊ฐ์ฒด์ ๋ถ๊ฐ ์ ๋ณด๋ฅผ ์ถ๊ฐํฉ๋๋ค.
      requestError.status = error.response.status;
      requestError.message = error.response.data.message;
      throw requestError;
    } else if (error.request) {
      // ์์ฒญ์ด ์ด๋ฃจ์ด ์ก์ผ๋ ์๋ต์ ๋ฐ์ง ๋ชปํจ
      console.log(error.request);
    } else {
      // ์ค๋ฅ๋ฅผ ๋ฐ์์ํจ ์์ฒญ์ ์ค์ ํ๋ ์ค์ ๋ฌธ์ ๊ฐ ๋ฐ์
      console.log('Error', error.message);
    }
  });
  return res?.data;
};
```

- http POST ์์ฒญ
```js
export const postDataFetcher = async (url, body) => {
  const res = await Axios.post(url, body).catch(function (error) {
      {/*์๋ต*/}
  });
  return res?.data;
};
```

- http PUT ์์ฒญ
```js
export const putDataFetcher = async (url, body) => {
  const res = await Axios.put(url, body).catch(function (error) {
      {/*์๋ต*/}
  });
  return res?.data;
};
```

- SWR ์ ์ญ ๋ฐ์ดํฐ ์ํ ๊ด๋ฆฌ fetcher
```js
export const localDataFetcher = (key) => {
  if (sessionStorage.getItem(key) === null) {
    return;
  } else {
    return JSON.parse(sessionStorage.getItem(key));
  }
};
```

### `isMobile.js`

- useragent์ ๋ฐ๋ผ์ boolean๊ฐ ๋ฐํ

```ts
export const isMobileOS = () => { ... ['android', 'iphone', 'ipad', 'ipod'] ...}
```

### `toast.js`

- props์ ๋ฐ๋ผ์ toast ํจ์ ๋ฐํ

```ts
const toastNotify = (props) => {
  const { state, message } = props;
    {/*์๋ต*/}

    return toast[state](message, {
      position: 'bottom-left',
      autoClose: 3000,
      hideProgressBar: false,
      closeOnClick: true,
      pauseOnHover: true,
      draggable: false,
    });
};
```

---

## โ Kaikas ์ฐ๋

- `UseKaikas.js` : Kaikas ์ฐ๋ํ์ฌ ํธ๋์ญ์ ์คํ

#### caver ๊ฐ์ฒด ์์ฑ ๋ฐ NFT ์ปจํธ๋ํธ ๊ฐ์ฒด ์์ฑ
```js
const caver = new Caver(window.klaytn);
const NFTContract = new caver.contract(NFTABI, NFT_ADDRESS);
const VoteContract = new caver.contract(VOTEABI, VOTE_ADDRESS);
```

#### Kaikas wallet ์ฐ๋
```js
export const kaikasLogin = async () => {
    const accounts = await window.klaytn.enable();
    const account = accounts[0]; 
    return account;
    {/*์๋ต*/}
};
```
#### Klaytn ๊ณ์  ์ฃผ์์ ์์ก์ ๋ฐํ
```js
export const kaikasGetBalance = async (address) => {
    const balance = await caver.rpc.klay.getBalance(address);
    return balance;
    {/*์๋ต*/}
};
```
#### ๋ฑ์ง๋ฐ NFT ๋ฐํ 
- ์ค๋งํธ ์ปจํธ๋ํธ ๋ด์์ ์ผ๋ฐ/๋ง์คํฐ NFT ๊ตฌ๋ถํด์ ๋ฐํ
- ํธ๋์ญ์์ result์์ event๋ฅผ ํ์ฑํ์ฌ ๋ง์คํฐ NFT DB์๋ฐ์ดํธ
- `caver.klay.sendTransaction` : ์ค๋งํธ ์ปจํธ๋ํธ ํธ๋์ญ์ ์คํ
- `estimateGas` : ํธ๋์ญ์ ์์ ๊ฐ์ค๋น ์ถ์ 
- `encodeABI` : ๋ฉ์๋์ ABI ์ธ์ฝ๋ฉ
- `decodeLog` : ABI ์ธ์ฝ๋ฉ๋ ๋ก๊ทธ ๋ฐ์ดํฐ ๋ฐ ์ธ๋ฑ์ฑ๋ ํ ํฝ ๋ฐ์ดํฐ๋ฅผ ๋์ฝ๋ฉ

```js
export const mintWithTokenURI = async ({
  tokenID,
  genralTokenURI,
  masterTokenURI,
  menuType,
  walletData,
  mintCountData,
  cid,
}) => {
  try {
    const estimatedGas = await NFTContract.methods
      .mintWithTokenURI(window.klaytn.selectedAddress, tokenID, genralTokenURI, masterTokenURI, menuType)
      .estimateGas({
        from: window.klaytn.selectedAddress,
      });

    const encodedData = NFTContract.methods
      .mintWithTokenURI(window.klaytn.selectedAddress, tokenID, genralTokenURI, masterTokenURI, menuType)
      .encodeABI();

    await caver.klay
      .sendTransaction({
        type: 'SMART_CONTRACT_EXECUTION',
        from: window.klaytn.selectedAddress,
        to: process.env.REACT_APP_NFT_CONTRACT_ADDRESS,
        data: encodedData,
        value: '0x00',
        gas: estimatedGas
      })
      .on('transactionHash', (hash) => {
        console.log(`transactionHash ${hash}`);
      })
      .on('receipt', (receipt) => {
        // success
        {/*...์๋ต. ํธ๋์ญ์์ด ์ฑ๊ณตํ๋ฉด ๊ทธ์ ๋ฐ๋ฅธ ๊ฐ์ข ํจ์ ์คํ */}

        const decodedMintMasterNFTeventLog = caver.klay.abi.decodeLog(
          [
            {
              indexed: false,
              name: 'typeString',
              type: 'string',
            },
          ], //"MintMasterNFT" ABI JSON interface
          receipt.logs[1].data,
          receipt.logs[1].topics.slice(1),
        );

        if (decodedMintMasterNFTeventLog?.typeString === 'MintMasterNFT') {
          //๋ง์คํฐ NFT ๋ฐํ ์ด๋ฒคํธ๋ฅผ ์บ์นํ๋ฉด ๋ง์คํฐ NFT DB์๋ฐ์ดํธ
          updateMintedMasterNft(cid);
        }
      })
      .on('error', (e) => {
        // failed
        {/*...์๋ต. ํธ๋์ญ์์ด ์คํจํ๋ฉด ๊ทธ์ ๋ฐ๋ฅธ ๊ฐ์ข ํจ์ ์คํ */}
      });
  } catch (error) {
    console.error('mintWithTokenURI', error);
  }
};
```

#### ๋ฉ๋ด ์ถ๊ฐ ์ ์ : Vote ์ปจํธ๋ํธ proposeMenu ๋ฉ์๋ ํธ์ถ

```js
export const proposeMenu = async (name) => {
  try {
    const estimatedGas = await VoteContract.methods.proposeMenu(name, NFT_ADDRESS).estimateGas({
      from: window.klaytn.selectedAddress,
    });
    const encodedData = VoteContract.methods.proposeMenu(name, NFT_ADDRESS).encodeABI();

    await caver.klay
      .sendTransaction({
        type: 'SMART_CONTRACT_EXECUTION',
        from: window.klaytn.selectedAddress,
        to: process.env.REACT_APP_VOTE_CONTRACT_ADDRESS,
        data: encodedData,
        value: '0x00',
        gas: estimatedGas,
      })
    {/*์๋ต*/}
  } catch (error) {
    console.error('proposeMenu', error);
  }
};
```

#### ๋ฉ๋ด ์ ์์ ํฌํ : Vote ์ปจํธ๋ํธ vote ๋ฉ์๋ ํธ์ถ

```js
export const vote = async (proposal) => {
  try {
    const estimatedGas = await VoteContract.methods.vote(proposal, NFT_ADDRESS).estimateGas();
    const encodedData = VoteContract.methods.vote(proposal, NFT_ADDRESS).encodeABI();
    await caver.klay
      .sendTransaction({
        type: 'SMART_CONTRACT_EXECUTION',
        from: window.klaytn.selectedAddress,
        to: process.env.REACT_APP_VOTE_CONTRACT_ADDRESS,
        data: encodedData,
        value: '0x00',
        gas: estimatedGas,
      })
    {/*์๋ต*/}
  } catch (error) {
    console.error('vote', error);
  }
};
```

#### ์ผ๋ฐ/๋ง์คํฐ NFT ํ๋ ๊ฒ์ฆ

- `isBadgemealNFTholder` : ์ผ๋ฐ NFT ํ๋์ด๋ฉด true, ์๋๋ฉด false ๋ฐํ
- `isBadgemealMasterNFTholder` : ๋ง์คํฐ NFT ํ๋์ด๋ฉด true, ์๋๋ฉด false ๋ฐํ

#### ๋ฉ๋ด ์ ์ ๋ฆฌ์คํธ ๋ฐ ์ฑํ๋ ๋ฉ๋ด ๋ฆฌ์คํธ ์กฐํ

- `getProposalList` : ์ผ๋ฐ NFT ํ๋์ด๋ฉด true, ์๋๋ฉด false ๋ฐํ
- `getWinnerProposalList` : ๋ง์คํฐ NFT ํ๋์ด๋ฉด true, ์๋๋ฉด false ๋ฐํ

--- 

## โ KAS ์ฐ๋

- `UseKas.js` : KAS ์ฐ๋ํ์ฌ ํธ๋์ญ์ ์คํ

#### KAS API ์ฌ์ฉํ๊ธฐ ์ํ option ๊ฐ์ฒด ์์ฑ
```js
const option = {
  headers: {
    Authorization:
      'Basic ' +
      Buffer.from(process.env.REACT_APP_ACCESS_KEY_ID + ':' + process.env.REACT_APP_SECRET_ACCESS_KEY).toString(
        'base64',
      ),
    'x-chain-id': process.env.REACT_APP_CHAIN_ID,
    'content-type': 'application/json',
  },
};
```

#### ํน์  EOA๊ฐ ๊ฐ์ง ๋ชจ๋  NFT ํ ํฐ ์ ๋ณด ์กฐํ
- ๊ธฐ๋ณธ๊ฐ 100๊ฐ๊น์ง ์กฐํ
```js
export const ownNftList = async (ownaddress) => {
  try {
    const response = await axios.get(
      `https://th-api.klaytnapi.com/v2/contract/nft/${process.env.REACT_APP_NFT_CONTRACT_ADDRESS}/owner/${ownaddress}`,
      option,
    );
    const data = response.data.items;
    {/*์๋ต*/}
  } catch (error) {
    console.log(error);
  }
};
```

#### ํน์  NFT ์ปจํธ๋ํธ์ ๋ชจ๋  ํ ํฐ ์ ๋ณด ์กฐํ
- ๊ธฐ๋ณธ๊ฐ 100๊ฐ๊น์ง ์กฐํ
```js
export const getNFTList = async () => {
  try {
    const response = await axios.get(
      `https://th-api.klaytnapi.com/v2/contract/nft/${process.env.REACT_APP_NFT_CONTRACT_ADDRESS}/token`,
      option,
    );
    const data = response.data.items;
    {/*์๋ต*/}
  } catch (error) {
    console.log(error);
  }
};
```

---

## โ Caver ์ฐ๋

- `UseCaverForOwner.js` : caver ๊ฐ์ฒด์ contract owner account ์ค์ ํ๊ณ  ํธ๋์ญ์ ์คํ

#### caver ๊ฐ์ฒด ์์ฑ ๋ฐ NFT ์ปจํธ๋ํธ ๊ฐ์ฒด ์์ฑ
```js
const caver = new Caver(new Caver.providers.HttpProvider('https://node-api.klaytnapi.com/v1/klaytn', option));
const NFTContract = new caver.contract(NFTABI, NFT_ADDRESS);
```

#### caver ๊ฐ์ฒด์ contract owner account ์ค์ 
```js
const deployer = caver.wallet.keyring.createFromPrivateKey(process.env.REACT_APP_DEPLOYER_PRIVATE_KEY);
caver.wallet.add(deployer);
```

#### ์ ์ ์๊ฒ ์์๋ก minter ๊ถํ ๋ถ์ฌ
```js
await NFTContract.methods.addBadgemealMinter(account).send({
      from: deployer.address, // owner ์ฃผ์
      gas: String(estimatedGas),
    });
```

#### ์ ์ ์ minter ๊ถํ ์ญ์ 
```js
await NFTContract.methods.removeBadgemealMinter(account).send({
      from: deployer.address, // owner ์ฃผ์
      gas: String(estimatedGas),
    });
```

---

## โ api ํจํค์ง

- api๋ฅผ ํธ์ถํ๊ณ  ์ ์ญ์ ์ผ๋ก ์ฌ์ฉํ๋ SWR ๋ฐ์ดํฐ๋ฅผ ๋ค๋ฃจ๋ ํจํค์ง

### [draw.js]

#### ๊ฐ์

- useDrawResultData : ํน์  ์ฃผ์์ ๋๋ค ๋ฝ๊ธฐ ๊ฒฐ๊ณผ ์ธ์ฆ ์ฌ๋ถ ์กฐํ
- useDrawMenuNumberData : ํน์  ์ฃผ์์ ๋๋ค ๋ฝ๊ธฐ ๋ฉ๋ด ๋ฒํธ ์กฐํ
- initDrawResult : ํน์  ์ฃผ์์ ๋๋ค ๋ฝ๊ธฐ ๊ฒฐ๊ณผ ์ด๊ธฐํ

#### Hook ์ฌ์ฉ๋ฒ

```ts
//useDrawResultData
const { drawResultData } = useDrawResultData(walletData?.account);

//useDrawMenuNumberData
const { menuNoData } = useDrawMenuNumberData(walletData?.account);

//initDrawResult
initDrawResult(walletData?.account);
```

### [menus.js]

#### ๊ฐ์

- useMenusData : ๋ฉ๋ด ๋ฆฌ์คํธ ์กฐํ

#### Hook ์ฌ์ฉ๋ฒ

```ts
const { menusData } = useMenusData();
```

### [mintData.js]

#### ๊ฐ์

- useMintData : ํด๋น ์ฃผ์์ ๋งคํ๋ mint data ์กฐํ
- initMintData : ํด๋น ์ฃผ์์ ๋งคํ๋ mint data ์ด๊ธฐํ

#### Hook ์ฌ์ฉ๋ฒ

```ts
//useMintData
const { menusData } = useMenusData();
//initMintData
initMintData(walletData?.account);
```

### [nft.js]

#### ๊ฐ์

- useMintCountData : ํด๋น ์ฃผ์์ ํ์ฌ ํ์ฐจ์ NFT ๋ฐ๊ธ ํ์ ์กฐํ
- updateMintCount : ํด๋น ์ฃผ์์ ํ์ฌ ํ์ฐจ์ NFT ๋ฐ๊ธ ํ์ ์์ 

#### Hook ์ฌ์ฉ๋ฒ

```ts
//useMintCountData
  const { mintCountData } = useMintCountData(walletData?.account);
//updateMintCount
updateMintCount(walletData?.account, mintCountData);
```

### [ipfs.js]

#### ๊ฐ์

- SWR๋ก ๊ด๋ฆฌํ์ง ์์.
- getMasterNftMetadataFetcher : ๋ง์คํฐ NFT ๋ฉํ๋ฐ์ดํฐ URL ์กฐํ
- updateMintedMasterNft : ํด๋น ๋ฉํ๋ฐ์ดํฐ์ NFT ๋ฐํ ์ฌ๋ถ ์๋ฐ์ดํธ

#### Hook ์ฌ์ฉ๋ฒ

```ts
//useDrawResultData
const { drawResultData } = useDrawResultData(walletData?.account);

//useDrawMenuNumberData
const { menuNoData } = useDrawMenuNumberData(walletData?.account);

//initDrawResult
initDrawResult(walletData?.account);
```
