# Substrate Kities

## 核心代码


https://github.com/zacksleo/substrate-kities-ui/blob/advanced-term-03/03_polkadot-js-api/kitties/frontend/src/Kitties.js#L16

```js


  // 猫咪的 DNA
  const [dnas, setDnas] = useState([])
  // 猫咪的所有者
  const [owners, setOwners] = useState([])

  // 格式化 DNA··
  const formatDna = (dna) => dna.isNone ? {} : dna.value.toU8a();

  const fetchKitties = () => {
    // TODO: 在这里调用 `api.query.kittiesModule.*` 函数去取得猫咪的信息。
    // 你需要取得：
    //   - 共有多少只猫咪
    //   - 每只猫咪的主人是谁
    //   - 每只猫咪的 DNA 是什么，用来组合出它的形态
    // 查询猫咪的数量

    let unsubCount = null;
    let unsubKitty = null;
    let unsubOwner = null;

    const asyncFetch = async () => {
      unsubCount = await api.query.kittiesModule.kittiesCount(async (res) => {
        const total = res.isNone ? 0 : res.value.toNumber();
        const indexes = [...Array(total).keys()];

        unsubKitty = await api.query.kittiesModule.kitties.multi(indexes, kitties => {
          setDnas(kitties.map((dna, id) => formatDna(dna)));
        });

        unsubOwner = await api.query.kittiesModule.owner.multi(indexes, owners => {
          setOwners(owners.map(owner => owner.toHuman()));
        });
      });
    }

    asyncFetch();

    return () => {
      unsubCount && unsubCount() && unsubKitty && unsubKitty() && unsubOwner && unsubOwner()
    }
  }

  const populateKitties = () => {
    // TODO: 在这里添加额外的逻辑。你需要组成这样的数组结构：
    //  ```javascript
    //  const kitties = [{
    //    id: 0,
    //    dna: ...,
    //    owner: ...
    //  }, { id: ..., dna: ..., owner: ... }]
    //  ```
    // 这个 kitties 会传入 <KittyCards/> 然后对每只猫咪进行处理
    setKitties(dnas.map((dna, id) => ({ id, dna, owner: owners[id] })))
  }

  useEffect(fetchKitties, [api, keyring])
  useEffect(populateKitties, [dnas, owners])

```

## Screenshots

## Alice create kities

![](./screenshots/alice-create-kities.png)

## Alice transfer kity to bob

![](./screenshots/alice-transfer-kity-to-bob.png)

### Kities of Bob

![](./screenshots/kities-of-bob.png)
