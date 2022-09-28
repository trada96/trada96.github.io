---
layout: post
title: Hướng dẫn viết smartcontract có thể upgrade ( upgradeable ) qua proxy sử dụng thư viện OpenZepplin
group: blockchain
state: publish
---

Bài viết này sẽ hướng dẫn chi tiết cách viết upgradebale smart contract cũng như giải thích về cách hoạt động của chúng.

Trong bài viết này, chúng ta sẽ có 7 tasks chi tiết, chúng có thể giúp bạn deploy proxy contract ở local testnet cũng như binance smart chain testnet ( tươn tự các chain khác). Mỗi task có thể có thêm nhiều các sub-tasks.

Chúng ta sử dụng [OpenZeppelin proxy contracts](https://docs.openzeppelin.com/contracts/4.x/api/proxy) và [OpenZeppelin Upgrades plugin for Hardhat](https://docs.openzeppelin.com/upgrades)

**Cách một upgradeable smart contract hoạt động như thế nào**

<p align="center">
  <img src="/images/blockchain/proxycontract/proxy_01.jpeg" />
</p>

[Source](https://res.cloudinary.com/practicaldev/image/fetch/s--nGhnsYS9--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_880/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/84bmjpjdb8d7rbc4szqc.png)

Minh hoạ này giải thích cách các hợp đồng thông minh có thể nâng cấp. Upgradeable smart contract bao gồm 3 contract sau:

- Proxy contract: Đây là smart contract mà người dùng ( end user) sẽ tương tác với. Nó sẽ lưu trữ data/state, có nghĩa là data sẽ được lưu trong smart contract proxy này. Đây là [proxy smart contract tiêu chuẩn](https://eips.ethereum.org/EIPS/eip-1967)

- Implementation contract: Smartcontract này sẽ cung cấp các hàm và logic chính. Các dữ liệu sẽ được defind trong hợp đồng này, và nó cũng là hợp đồng chính chúng ta cần xây dựng.

- ProxyAdmin contract: Hơp đông này sẽ liên kết giữa Proxy và Implementation.

ProxyAdmin đã được giải thích chi tiết tại [OpenZeppelin docs](https://docs.openzeppelin.com/upgrades-plugins/1.x/faq#what-is-a-proxy-admin)

Nếu bạn chuyển giao ProxyAdmin ownership đến một tài khoản multi-sig thì quyền nâng cấp smartcontract sẽ được chuyển qua cho địa chỉ đó.

**Làm thế nào để triển khai Proxy? Làm thế nào để nâng cấp proxy**

Khi bạn triển khai upgradeale contract sử dụng OpenZeppelin Upgrades plugin của HardHat, chúng ta sẽ triển khai 3 contract:

1. Triển khai "Implementation contract"
2. Triển khai "ProxyAdmin 3. contract"
3. Triển khai "Proxy contract"

Trong ProxyAdmin Contract, Implementation và Proxy được liên kết với nhau.

Khi một user gọi đến proxy contract, lời gọi ( the call from user) sẽ được uỷ quyền (delegate) tới implementation contract ( [delegate call](https://docs.soliditylang.org/en/v0.8.12/introduction-to-smart-contracts.html?highlight=delegatecall#delegatecall-callcode-and-libraries))

**_Khi nâng cấp một contract, những gì chúng ta cần làm là:_**

1. Triển khai 1 "Implementation contract" mới
2. Upgrade trong "ProxyAdmin contract" bằng cách chuyển hướng tất cả các lời gọi cho "Implementation contract" mới.

# TASK 1

**Task1: Write upgradeable smart contract**

**_Task1.1: init hardhat project_**

Chúng ta sẽ sử dụng Hardhat, Hardhat local testnet và Openzepplin Plugin.

Bước 1: Cài đặt hardhat và khởi tạo project

```
mkir SOLIDITY_PROXY && cd SOLIDITY_PROXY

yarn init -y
yarn add hardhat

yarn hardhat
// choose option: sample typescript
```

Bước 2: Thêm plugin @openzeppelin/hardhat-upgrades

```
yarn add @openzeppelin/hardhat-upgrades
```

Edit hardhat.config.ts để sử dụng Upgraded plugins.

```
// hardhat.config.ts
import '@openzeppelin/hardhat-upgrades';
```

Chúng ta sẽ sử dụng 3 hàm của hardhat

- deployProxy()
- upgradeProxy()
- prepareUpgrade()

**_Task1.2: Viết một upgradeable smart contract_**

Chúng ta sẽ sử dụng contract Box.sol từ [OpenZeppelin learn guide](https://docs.openzeppelin.com/learn/developing-smart-contracts#setting-up-a-solidity-project)
Chúng ta sẽ cùng nhau xây các version của contract này

- Box.sol
- BoxV2.sol
- BoxV3.sol
- BoxV4.sol

Sự khác nhau lớn của contract bình thường và upgradeable là upgradeable contract không có **contructor** [docs link](https://docs.openzeppelin.com/upgrades-plugins/1.x/writing-upgradeable)

```
// contracts/Box.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract Box {
    uint256 private value;

    // Emitted when the stored value changes
    event ValueChanged(uint256 newValue);

    // Stores a new value in the contract
    function store(uint256 newValue) public {
        value = newValue;
        emit ValueChanged(newValue);
    }

    // Reads the last stored value
    function retrieve() public view returns (uint256) {
        return value;
    }
}

```

User có thể store() và retrieve một giá trị bất kì nào đó.

**_Task1.3 Viết unit test cho Box.sol_**

Bây giờ cùng viết một unit test cho contract Box.sol.

Tạo mới ( sửa nếu đã có) test/1.Box.test.ts

```
// test/1.Box.test.ts
import { expect } from "chai";
import { ethers } from "hardhat"
import { Contract, BigNumber } from "ethers"

describe("Box", function () {
  let box:Contract;

  beforeEach(async function () {
    const Box = await ethers.getContractFactory("Box")
    box = await Box.deploy()
    await box.deployed()
  })

  it("should retrieve value previously stored", async function () {
    await box.store(42)
    expect(await box.retrieve()).to.equal(BigNumber.from('42'))

    await box.store(100)
    expect(await box.retrieve()).to.equal(BigNumber.from('100'))
  })
})

// NOTE: should also add test for event: event ValueChanged(uint256 newValue)

```

Run test:

```
yarn hardhat test test/1.Box.test.ts
```

Kết quả:

```
  Box
    ✓ should retrieve value previously stored
    1 passing (505ms)
  ✨  Done in 3.34s.
```

# Task 2

**Task2: Triển khai upgradeable smart contract**

**_Task2.1 Viết script triển khai contract bằng hardhat_**

Khi viết một script để triển khai smart contract, ta có thể viết như sau:

```
const Greeter = await ethers.getContractFactory("Greeter");
const greeter = await Greeter.deploy("Hello, Hardhat!");
```

Để triển khai một upgradeable contract, cần gọi hàm deployProxy() để thay thế. Đọc thêm tại [Docs](https://docs.openzeppelin.com/upgrades-plugins/1.x/api-hardhat-upgrades#deploy-proxy)

```
const Box = await ethers.getContractFactory("Box")
const box = await upgrades.deployProxy(Box,[42], { initializer: 'store' })
```

Tại dòng thứ 2, chúng ta sử dụng Openzepplin Upgrades Plugin để deploy contract Box với giá trị khởi tạo là 42 được gọi bỏi hàm store như một initializer.

Sửa file "scripts/1.deploy_box.ts"

```
// scripts/1.deploy_box.ts
import { ethers } from "hardhat"
import { upgrades } from "hardhat"

async function main() {

  const Box = await ethers.getContractFactory("Box")
  console.log("Deploying Box...")
  const box = await upgrades.deployProxy(Box,[42], { initializer: 'store' })

  console.log(box.address," box(proxy) address")
  console.log(await upgrades.erc1967.getImplementationAddress(box.address)," getImplementationAddress")
  console.log(await upgrades.erc1967.getAdminAddress(box.address)," getAdminAddress")
}

main().catch((error) => {
  console.error(error)
  process.exitCode = 1
})
```

Giải thích:

- Chúng ta deploy một upgradeable contract Box.sol qua upgrades.deployProxy().
- 3 contract được deploy: Implementation, ProxyAdmin, Proxy. Chúng ta có thể log ra địa chỉ của các contract này như trên.

**_Task2.2: Triển khai smartcontract trên local testnet_**

STEP 1: Chạy một harhad testnet trên một terminal khác:

```
  yarn hardhat node
```

STEP 2: Chạy deploy script

```
  yarn hardhat run scripts/1.deploy_box.ts --network localhost
```

Kết quả:

```
  Deploying Box...
  0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0  box(proxy) address
  0x5FbDB2315678afecb367f032d93F642f64180aa3  getImplementationAddress
  0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512  getAdminAddress
  ✨  Done in 3.83s.
```

Bạn có thể tương tác với box contract thông qua proxy address: 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0

Lưu ý: Nếu bạn triển khai nhiều lần khác nhau, bạn thấy ProxyAdmin luôn giống nhau: 0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512

**_Task2.3: Kiểm tra xem Box.sol sử dụng proxy có hoạt động chính xác không_**

Các User tương tác với implementation contract thông qua proxy contract.

Để chắc chắn rằng nó hoạt động đúng như kì vọng không, giờ ta cần thêm một unit test. Trong unit test này, chúng ta triển khai contract sử dụng upgrades.deployProxy() và tương tác thông qua proxy contract.

Chỉnh sủa file "test/2.BoxProxy.test.ts"

```
// test/2.BoxProxy.test.ts
import { expect } from "chai"
import { ethers, upgrades } from "hardhat"
import { Contract, BigNumber } from "ethers"

describe("Box (proxy)", function () {
  let box:Contract

  beforeEach(async function () {
    const Box = await ethers.getContractFactory("Box")
    //initialize with 42
    box = await upgrades.deployProxy(Box, [42], {initializer: 'store'})
    })

  it("should retrieve value previously stored", async function () {
    // console.log(box.address," box(proxy)")
    // console.log(await upgrades.erc1967.getImplementationAddress(box.address)," getImplementationAddress")
    // console.log(await upgrades.erc1967.getAdminAddress(box.address), " getAdminAddress")

    expect(await box.retrieve()).to.equal(BigNumber.from('42'))

    await box.store(100)
    expect(await box.retrieve()).to.equal(BigNumber.from('100'))
  })

})
```

Run:

```
  yarn hardhat test test/2.BoxProxy.test.ts
```

Kết quả:

```
  Box (proxy)
    ✓ should retrieve value previously stored
  1 passing (579ms)
  ✨  Done in 3.12s.`
```

Contract đã chạy như mong đợi.

Bây giờ, ta cần thêm 1 function mới là increment(). Thay vì deploy lại contract này, migrate dũ liệu sang contract mới và yêu cầu tất cả users truy cập vào địa chỉ hợp đồng mới. Ta có thể làm dễ dàng hơn vơi proxy pattern.

# Task 3

**Task3: Upgrade contract sang BoxV2**
**_Task3.1: Viết một implementation contract mới_**

Chúng ta sẽ viết một version mới của Box contract tên là BoxV2.sol kế thưa Box.sol ( hoặc có thể viết mới tuỳ thích)

Chỉnh sủa "contracts/BoxV2.sol"

```
// contracts/BoxV2.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./Box.sol";

contract BoxV2 is Box{
    // Increments the stored value by 1
    function increment() public {
        store(retrieve()+1);
    }
}
```

**_Task3.2: Test script cho việc triển khai thông thường_**

Chúng ta viết một unit test để test BoxV2 sau khi deploy trên local.

Chỉnh sửa "test/3.BoxV2.test.ts"

```
// test/3.BoxV2.test.ts
import { expect } from "chai"
import { ethers } from "hardhat"
import { Contract, BigNumber } from "ethers"

describe("Box V2", function () {
  let boxV2:Contract

  beforeEach(async function () {
    const BoxV2 = await ethers.getContractFactory("BoxV2")
    boxV2 = await BoxV2.deploy()
    await boxV2.deployed()
  });

  it("should retrieve value previously stored", async function () {
    await boxV2.store(42)
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('42'))

    await boxV2.store(100)
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('100'))
  });

  it('should increment value correctly', async function () {
    await boxV2.store(42)
    await boxV2.increment()
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('43'))
  })

})

```

Run test:

```
  yarn hardhat test test/3.BoxV2.test.ts
```

Kết quả:

```
  Box V2
      ✓ should retrieve value previously stored
      ✓ should increment value correctly
    2 passing (579ms)
✨  Done in 3.38s.
```

Vậy là contract V2 chạy tốt, không vấn đề gì cả.

**_Task3.3: Viết test script cho việc triển khai upgrade contract_**

Chúng ta sẽ viết unit test cho BoxV2 được deploy bởi Proxy.

- Bước 1: deploy Box.sol
- Bước 2: Upgrade nó lên BoxV2.sol
- Bước 3: Test xem BoxV2 có hoạt động chính xác hay không?

Chỉnh sửa "test/4.BoxProxyV2.test.ts":

```
// test/4.BoxProxyV2.test.ts
import { expect } from "chai"
import { ethers, upgrades } from "hardhat"
import { Contract, BigNumber } from "ethers"

describe("Box (proxy) V2", function () {
  let box:Contract
  let boxV2:Contract

  beforeEach(async function () {
    const Box = await ethers.getContractFactory("Box")
    const BoxV2 = await ethers.getContractFactory("BoxV2")

    //initilize with 42
    box = await upgrades.deployProxy(Box, [42], {initializer: 'store'})
    // console.log(box.address," box/proxy")
    // console.log(await upgrades.erc1967.getImplementationAddress(box.address)," getImplementationAddress")
    // console.log(await upgrades.erc1967.getAdminAddress(box.address), " getAdminAddress")

    boxV2 = await upgrades.upgradeProxy(box.address, BoxV2)
    // console.log(boxV2.address," box/proxy after upgrade")
    // console.log(await upgrades.erc1967.getImplementationAddress(boxV2.address)," getImplementationAddress after upgrade")
    // console.log(await upgrades.erc1967.getAdminAddress(boxV2.address)," getAdminAddress after upgrade")
  })

  it("should retrieve value previously stored and increment correctly", async function () {
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('42'))

    await boxV2.increment()
    //result = 42 + 1 = 43
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('43'))

    await boxV2.store(100)
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('100'))
  })

})
```

Run test:

```
  yarn hardhat test test/4.BoxProxyV2.test.ts
```

Kết quả:

```
  Box (proxy) V2
      ✓ should retrieve value previously stored and increment correctly


    1 passing (617ms)

  ✨  Done in 3.44s.

```

**_Task3.4 Viết upgrade script_**

Trong sub-task 2.2, chúng ta đã deploy Proxy(Box) tới 0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0

Trong sub-task này, chúng ta sẽ upgrade nó tới BoxV2 ( deploy contract mới, liên kết proxy tới contract mới thông qua proxyAdmin)

Chỉnh sủa "scripts/2.upgradeV2.ts"

```
// scripts/2.upgradeV2.ts
import { ethers } from "hardhat";
import { upgrades } from "hardhat";

const proxyAddress = '0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0'

async function main() {
  console.log(proxyAddress," original Box(proxy) address")
  const BoxV2 = await ethers.getContractFactory("BoxV2")
  console.log("upgrade to BoxV2...")
  const boxV2 = await upgrades.upgradeProxy(proxyAddress, BoxV2)
  console.log(boxV2.address," BoxV2 address(should be the same)")

  console.log(await upgrades.erc1967.getImplementationAddress(boxV2.address)," getImplementationAddress")
  console.log(await upgrades.erc1967.getAdminAddress(boxV2.address), " getAdminAddress")
}

main().catch((error) => {
  console.error(error)
  process.exitCode = 1
})
```

**_Task3.5 Chạy upgrade script_**
Chúng ta sẽ bắt đầu lại từ đầu.

STEP1: Chạy một mạng local testnet ở một terminal khác

```
yarn hardhat node
```

STEP2: Deploy Box V1

```
  yarn hardhat run scripts/1.deploy_box.ts --network localhost
```

Proxy(Box) sẽ đuọc triển khai với địa chỉ: 0x9fE46736679d2D9a65F0992F2272dE9f3c7fa6e0

STEP3: Upgrade sang BoxV2

```
  yarn hardhat run scripts/2.upgradeV2.ts --network localhost
```

Kết quả:

```
  0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0  original Box(proxy) address
  upgrade to BoxV2...
  0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0  BoxV2 address(should be the same)
  0x5FC8d32690cc91D4c39d9d3abcBD16989F875707  getImplementationAddress
  0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512  getAdminAddress
  ✨  Done in 3.64s.
```

# Task 4

**Task 4: Tương tác với contract bằng hardhat console**
**_Task4.1: Tương tác với proxy contract_**

Chạy hardhat console để kết nối với local testnet

```
  yarn hardhat console --network localhost
```

Trong hardhat console

```
address = '0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0'
boxv2 = await ethers.getContractAt("BoxV2", address)
await boxv2.retrieve()
//BigNumber { value: "42" }

await boxv2.increment()
// tx response
// {
// hash: '0x3e8c9dd8842d3315cadad2a80b592ac369e644edc5cec16f7a22c76d49e4b921',
// blockNumber: 6,

await boxv2.retrieve()
//BigNumber { value: "43" }

await boxv2.store(100)
// tx response ...
await boxv2.retrieve()
//BigNumber { value: "100" }
```

**_Task4.2 Thử tương tác với implementation contract_**

```
addressimp = '0x5fc8d32690cc91d4c39d9d3abcbd16989f875707'
boximp = await ethers.getContractAt("BoxV2", addressimp)

await boximp.retrieve()
//BigNumber { value: "0" }
await boximp.increment()
// tx response ...
await boximp.retrieve()
BigNumber { value: "1" }
```

Chúng ta có thể thấy rằng, giá trị ban đầu trong implementation contract bằng 0. Đó là vì data được lưu trong context của Proxy contract.

# Task 5

**Task5: Viết contract BoxV3 với thêm một biến mới**

Chúng ta sẽ thêm một biến trạng thái mới miến là không thay đổi gì với các biến trạng thái đã có.

Để đơn giản, chúng ta sẽ add một `string public name`

**_Task5.1: Viết BoxV3.sol_**

Chúng ta sẽ viết BoxV3 kế thừa BoxV2

```
// contracts/BoxV3.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./BoxV2.sol";

contract BoxV3 is BoxV2{
    string public name;

    event NameChanged(string name);
    function setName(string memory _name) public {
        name = _name;
        emit NameChanged(name);
    }
}
```

**_Task5.2: Viết test cho việc triển khai proxy_**

Chỉnh sửa "test/5.BoxProxyV3.test.ts"

```
// test/5.BoxProxyV3.test.ts
import { expect } from "chai"
import { ethers, upgrades } from "hardhat"
import { Contract, BigNumber } from "ethers"

describe("Box (proxy) V3 with name", function () {
  let box:Contract
  let boxV2:Contract
  let boxV3:Contract

  beforeEach(async function () {
    const Box = await ethers.getContractFactory("Box")
    const BoxV2 = await ethers.getContractFactory("BoxV2")
    const BoxV3 =  await ethers.getContractFactory("BoxV3")

    //initialize with 42
    box = await upgrades.deployProxy(Box, [42], {initializer: 'store'})
    boxV2 = await upgrades.upgradeProxy(box.address, BoxV2)
    boxV3 = await upgrades.upgradeProxy(box.address, BoxV3)
  })

  it("should retrieve value previously stored and increment correctly", async function () {
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('42'))
    await boxV3.increment()
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('43'))

    await boxV2.store(100)
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('100'))
  })

  it("should set name correctly in V3", async function () {
    expect(await boxV3.name()).to.equal("")

    const boxname="my Box V3"
    await boxV3.setName(boxname)
    expect(await boxV3.name()).to.equal(boxname)
  })

})

// NOTE: should also add test for event: event NameChanged(string name)

```

Run Test:

```
  yarn hardhat test test/5.BoxProxyV3.test.ts
```

Kết quả:

```
    Box (proxy) V3 with name
      ✓ should retrieve value previously stored and increment correctly
      ✓ should set name correctly in V3
    2 passing (748ms)
  ✨  Done in 3.12s.
```

**_Task5.3: Viết upgrade script_**

Chỉnh sửa: "scripts/3.upgradeV3.ts"

```
// scripts/3.upgradeV3.ts
import { ethers } from "hardhat";
import { upgrades } from "hardhat";

const proxyAddress = '0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0'
// const proxyAddress = '0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268'
async function main() {
  console.log(proxyAddress," original Box(proxy) address")
  const BoxV3 = await ethers.getContractFactory("BoxV3")
  console.log("upgrade to BoxV3...")
  const boxV3 = await upgrades.upgradeProxy(proxyAddress, BoxV3)
  console.log(boxV3.address," BoxV3 address(should be the same)")

  console.log(await upgrades.erc1967.getImplementationAddress(boxV3.address)," getImplementationAddress")
  console.log(await upgrades.erc1967.getAdminAddress(boxV3.address), " getAdminAddress")
}

  main().catch((error) => {
    console.error(error)
    process.exitCode = 1
  })

```

**_Task5.4: Deploy và tương tác với BoxV3_**

STEP 1: Upgrade lên BoxV3

```
  yarn hardhat run scripts/3.upgradeV3.ts --network localhost
```

Kết quả:

```
  0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0  original Box(proxy) address upgrade to BoxV3...
  0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0  BoxV3 address(should be the same)
  0xa513E6E4b8f2a923D98304ec87F64353C4D5C853  getImplementationAddress
  0xe7f1725E7734CE288F8367e1Bb143E90bb3F0512  getAdminAddress
    ✨  Done in 3.52s.
```

STEP 2: Tương tác với BoxV3 trên hardhat console

```
  yarn hardhat console --network localhost
```

```
address = '0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0'
boxv3 = await ethers.getContractAt("BoxV3", address)
await boxv3.retrieve()
//BigNumber { value: "42" }

await boxv3.setName("mybox")
// tx response
await boxv3.name()
//'mybox'
```

# Task 6

**Task6: Viết BoxV4 với prepareUpgrade**

Chúng ta sẽ viết BoxV4 và chuẩn bị upgrade cho Task7 để deploy và upgrade 3 version của Box contract lên Ropsten.

Viết Boxv4.sol, unit test và upgrade script

**_Task6.1: Viết BoxV4.sol_**

Chúng ta sẽ viết BoxV4 có tương tác với biến `name `

- Chúng ta thay đổi biến trạng thái `name` từ public sang private
- Khi chúng ta getName(), một prefix sẽ được thêm

```
// contracts/BoxV4.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "./BoxV2.sol";

contract BoxV4 is BoxV2{
    string private name;

    event NameChanged(string name);
    function setName(string memory _name) public {
        name = _name;
        emit NameChanged(name);
    }

  function getName() public view returns(string memory){
      return string(abi.encodePacked("Name: ",name));
    }
}
```

**_Task6.2: Viết test cho proxy deployment_**

Chỉnh sủa "test/6.BoxProxyV4.test.ts"

```
// test/6.BoxProxyV4.test.ts
import { expect } from "chai"
import { ethers, upgrades } from "hardhat"
import { Contract, BigNumber } from "ethers"

describe("Box (proxy) V4 with getName", function () {
  let box:Contract
  let boxV2:Contract
  let boxV3:Contract
  let boxV4:Contract

  beforeEach(async function () {
    const Box = await ethers.getContractFactory("Box")
    const BoxV2 = await ethers.getContractFactory("BoxV2")
    const BoxV3 =  await ethers.getContractFactory("BoxV3")
    const BoxV4 =  await ethers.getContractFactory("BoxV4")

    //initialize with 42
    box = await upgrades.deployProxy(Box, [42], {initializer: 'store'})
    boxV2 = await upgrades.upgradeProxy(box.address, BoxV2)
    boxV3 = await upgrades.upgradeProxy(box.address, BoxV3)
    boxV4 = await upgrades.upgradeProxy(box.address, BoxV4)
  })

  it("should retrieve value previously stored and increment correctly", async function () {
    expect(await boxV4.retrieve()).to.equal(BigNumber.from('42'))
    await boxV4.increment()
    expect(await boxV4.retrieve()).to.equal(BigNumber.from('43'))

    await boxV2.store(100)
    expect(await boxV2.retrieve()).to.equal(BigNumber.from('100'))
  })

  it("should setName and getName correctly in V4", async function () {
    //name() removed, getName() now
    // expect(boxV4).to.not.have.own.property("name")
    expect(boxV4.name).to.be.undefined
    expect(await boxV4.getName()).to.equal("Name: ")

    const boxname="my Box V4"
    await boxV4.setName(boxname)
    expect(await boxV4.getName()).to.equal("Name: "+boxname)
  })

})
```

Run Test:

```
  yarn hardhat test test/6.BoxProxyV4.test.ts
```

Kết quả:

```
Box (proxy) V4 with getName
    ✓ should retrieve value previously stored and increment correctly
    ✓ should setName and getName correctly in V4
  2 passing (771ms)
✨  Done in 2.46s.
```

**_Task 6.3: viết script để chuẩn bị upgrade_**
Chúng ta sẽ viết script này sử dụng `upgrades.prepareUpgrade`

Khi gọi hàm `upgrades.upgradeProxy()` có 2 job sẽ được thực hiện

- Implementation contract được triển khai
- ProxyAdmin upgrade() sẽ đươc gọi để liên kế Proxy với implementation contract.

Khi gọi `upgrades.prepareUpgrade()` chỉ có job đầu tiên được thực hiện, job thứ 2 sẽ được để lại cho developers thực hiện thủ công

Chỉnh sửa "scripts/4.prepareV4.ts"

```
// scripts/4.prepareV4.ts
import { ethers } from "hardhat";
import { upgrades } from "hardhat";

const proxyAddress = '0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0'
// const proxyAddress = '0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268'
async function main() {
  console.log(proxyAddress," original Box(proxy) address")
  const BoxV4 = await ethers.getContractFactory("BoxV4")
  console.log("Preparing upgrade to BoxV4...");
  const boxV4Address = await upgrades.prepareUpgrade(proxyAddress, BoxV4);
  console.log(boxV4Address, " BoxV4 implementation contract address")
}

main().catch((error) => {
  console.error(error)
  process.exitCode = 1
})
```

Run deployment:

```
  yarn hardhat run scripts/4.prepareV4.ts --network localhost
```

Kết quả:

```
  0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0  original Box(proxy) address
  Preparing upgrade to BoxV4...
  0x610178dA211FEF7D417bC0e6FeD39F05609AD788  BoxV4 implementation contract address
  ✨  Done in 3.90s.
```

Bạn có thể tương tác với proxy contract (0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0) trong hardhat console bới
`yarn hardhat console --network localhost`

```
address = '0x9fe46736679d2d9a65f0992f2272de9f3c7fa6e0'
boxv4 = await ethers.getContractAt("BoxV4", address)

await boxv4.getName()
```

# Task 7

**Task7: Deploy Box lên mạng Ropsten**
**_Task7.1: Chuẩn bị deploy lên Ropsten_**
Chúng ta có thể deploy smart contract lên Ropsten trực tiếp sử dụng Hardhat.

STEP 1: Chỉnh sủa Alchemy/Infura endpoint trong `.env`

```
ROPSTEN_URL=https://eth-ropsten.alchemyapi.io/v2/{YOURS}
PRIVATE_KEY={YOURS HERE}
```

STEP2: Chắc chắn bạn có Ropsten trong `hardhat.config.ts`

```
networks: {
    ropsten: {
      url: process.env.ROPSTEN_URL || "",
      accounts:
        process.env.PRIVATE_KEY !== undefined ? [process.env.PRIVATE_KEY] : [],
    },
  },
```

**_Task7.2: Deploy BoxV1_**

```
yarn hardhat run scripts/1.deploy_box.ts --network ropsten
```

<p align="center">
  <img src="/images/blockchain/proxycontract/ropsten_01.png" />
</p>

3 contract đã được deploy:

- 0x7fcb5f0898ee1394c6cb44e3a62b9e9fc19d0e1c, implementation contract
- 0x9ce0fdc88df321c804ed0ad9cefe87d97a30479e, ProxyAmin contract
- 0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268, proxy contract

Bạn có thể tương tác với proxy contract từ hardhat console qua `yarn hardhat console --network ropsten`

Trong console:

```
address = '0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268'
box = await ethers.getContractAt("Box", address)
await box.retrieve()
```

**_Task7.3: Upgrade lên Boxv2_**

Run:

```
yarn hardhat run scripts/2.upgradeV2.ts --network ropsten
```

Script trên sẽ thực thi 2 jobs:

- Triển khai implementation contract mới
- Gọi ProxyAdmin.upgrade()

<p align="center">
  <img src="/images/blockchain/proxycontract/ropsten_02.png" />
</p>

Có thể tương tác với Proxy contract qua console:

```
address = '0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268'
box = await ethers.getContractAt("BoxV2", address)

await box.increment()
//wait for tx to be mined in a new block...

await box.retrieve()
//BigNumber { value: "44" }
```

**_Task7.4: Upgrade lên BoxV3_**
Run

```
yarn hardhat run scripts/3.upgradeV3.ts --network ropsten
```

Tương tác với Proxy và BoxV3 trong console:

```
address = '0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268'
box = await ethers.getContractAt("BoxV3", address)

box = await ethers.getContractAt("BoxV3", address)
await box.setName("mybox")
```

<p align="center">
  <img src="/images/blockchain/proxycontract/ropsten_03.png" />
</p>

**_Task 7.5: chuẩn bị upgrade lên Box V4_**

Run:

```
yarn hardhat run scripts/4.prepareV4.ts --network ropsten
```

Script này sẽ deploy lên BoxV4 contract at:
`0xA0726E6e045f84dEe8D7cA4CdD427A68dd336458`

Bạn có thể upgrade nó trực tieos tại `https://ropsten.etherscan.io`

ProxyAdmin đã được verify trên Etherscan block explorer. Trong ProxyAdmin write contract page, connect tới ví và chạy upgrade() giống như bên dưới

<p align="center">
  <img src="/images/blockchain/proxycontract/ropsten_04.png" />
</p>

```
address = '0x1CD0c84b7C7C1350d203677Bb22037A92Cc7e268'
box = await ethers.getContractAt("BoxV4", address)

await box.getName()
//'Name: mynewbox'
```

**_Task7.6: Đọc ProxyAdmin contract_**
Bạn cũng có thể đọc ProxyAdmin contract để lấy thông tin về Proxy contract:

- getProxyAdmin
- getProxyImplementation
- owner

<p align="center">
  <img src="/images/blockchain/proxycontract/ropsten_05.png" />
</p>

Nếu bạn muốn verify Box, BoxV2, BoxV3, BoxV4 trên Etherscan, ta có thể sư dụng plugin của hardhat

Install

```
yarn add @nomiclabs/hardhat-etherscan
```

Verify BoxV4

```
yarn hardhat verify 0xA0726E6e045f84dEe8D7cA4CdD427A68dd336458 --network ropsten

```

Trên đây là tất cả để bạn có thể tạo một Proxy Contract

# Tài Liệu

- https://dev.to/yakult/tutorial-write-upgradeable-smart-contract-proxy-contract-with-openzeppelin-1916
- https://docs.openzeppelin.com/learn/upgrading-smart-contracts#upgrading
- https://docs.openzeppelin.com/defender/guide-upgrades
- https://docs.openzeppelin.com/upgrades-plugins/1.x/proxies
- https://www.preethikasireddy.com/post/the-hardest-concept-for-developers-to-grasp-about-web-3-0
- https://forum.openzeppelin.com/t/openzeppelin-upgrades-step-by-step-tutorial-for-hardhat/3580
- https://blog.openzeppelin.com/proxy-patterns/
- https://blog.trailofbits.com/2018/09/05/contract-upgrade-anti-patterns/
- https://blog.openzeppelin.com/the-state-of-smart-contract-upgrades/
