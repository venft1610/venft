<template>
  <div class='hello'>
      <h1>{{ msg }}</h1>
      <h1 style='color:red'>{{msg_err}}</h1>
      <h2>mnemonic: </h2> <span>{{ mnemonic }}</span>

      <p>----------------------------------------------</p>
      <div class='row'>
          <div class='col-sm-12 border p-2'>
              <h2>public key</h2>
              <p>{{ public_key }}</p>
              <button class='btn btn-primary' v-on:click='CreatePublicKey()'>Create Public Key</button>
          </div>
      </div>
      <p>----------------------------------------------</p>
      <p>
          Tạo địa chỉ tiền gửi tài khoản Polygon từ Khóa công khai mở rộng.
          <br />
          Địa chỉ tiền gửi được tạo cho chỉ mục cụ thể - mỗi khóa công khai mở rộng có thể tạo tối đa 2 ^ 31 địa chỉ bắt đầu từ chỉ mục 0 cho đến 2 ^ 31.
      </p>

      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <p> address 1: {{ address }} </p>
              <p>address 2: {{ address_2 }}</p>
              <button class='btn btn-primary' v-on:click='CreateAcountPolygon()'>Create Account Polygon</button>
          </div>
          <div class='col-sm-6 border p-2'>
              <h2>private key </h2>
              <p> {{ private_key }}</p>
              <button class='btn btn-primary' v-on:click='CreatePrivateKey()'>Create Private Key Polygon</button>
          </div>
      </div>
      <p>----------------------------------------------</p>
      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <h2>   current block number: {{ current_block_number }}</h2>
              <button class='btn btn-primary' v-on:click='GetCurrentBlockNumber()'>Get current block number</button>
          </div>
          <div class='col-sm-6 border p-2'>
              <h2>Account balance: {{ account_balance }}</h2>
              <span>Address </span> <input type='text' class='form mb-1' style='width:80%' v-model='address' />
              <br />
              <button class='btn btn-primary' v-on:click='GetPolygonAccountBalance()'>Get account balance</button>
          </div>
      </div>
      <p>----------------------------------------------</p>
      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <p> hash: {{ hash }}</p>
              <p>block by hash {{block_by_hash}}</p>
              <button class='btn btn-primary' v-on:click='GetBlockByHash()'>Get block by hash</button>
          </div>
          <div class='col-sm-6 border p-2'>
              <h2> polygon transaction by hash: {{ polygon_transation }}</h2>
              <button class='btn btn-primary' v-on:click='GetPolygonTransaction()'>Get polygon transation</button>
          </div>

      </div>

      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <h2> polygon transaction by adddres : {{ block_by_addres }}</h2>
              <button class='btn btn-primary' v-on:click='GetPolygonTransactionByAddress()'>Get polygon transation by addres</button>
          </div>
          <div class='col-sm-6 border p-2'>
              <h2> Get Virtual accounts : {{ vitual_account }}</h2>
              <button class='btn btn-primary' v-on:click='GetVirtualAccounts()'>Get virtual accounts</button>
          </div>
      </div>

      <p>----------------------------------------------</p>
      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <h4>Send Matic To Address</h4>
              <span>Address </span> <input type='text' class='form mb-1' style='width:70%' v-model='address' />
              <br /><span>Current Balance </span> <input type='text' class='form mb-1' style='width:70%' v-model='account_balance' />

              <br />
              <button class='btn btn-primary' v-on:click='SendMaticToAddress()'>Send Matic To Address</button>
              <p>hash of transaction: {{hash_of_transaction}}</p>
          </div>
          <div class='col-sm-6 border p-2'>
              <h4>MIN NFT</h4>
              <span>IPFS CID </span> <input type='text' class='form mb-1' style='width:70%' v-model='meta_data_hash_result' />
              <p> txId: {{ txid_min}}</p>
              <button class='btn btn-primary' v-on:click='MinNFT()'>MinNFT</button>
          </div>
      </div>
      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <h4>upload image to IPFS</h4>
              <input type='file' name='file' @change='changeFile' />
              <p>image name: {{img_name}}</p>
              <p>ipfs Hash: {{ipfs_hash}}</p>
              <button class='btn btn-primary' v-on:click='UploadFileToIPFS()'>Upload File To IPFS</button>
              <p>ipfs hash test: bafkreiev5hsjrxustrjgsgg5ctwq7qfqy2dhd5wew6353wjxwb4o5rknaa</p>
              <img v-if="ipfs_hash!=''" :src='"https://" + ipfs_hash + ".ipfs.nftstorage.link"'  style='width:50%'/>
          </div>
          <div class='col-sm-3 border p-2'>
              <h4>upload meta data</h4>
              <span>chọn meta data (file định dạng json) </span><input type='file' name='file' @change='changeFile' />
              <br /><span>json demo</span><input type='text' class='form mb-1' style='width:70%' value="{ 'name': 'Name', 'description': 'Description', 'image': 'ipfs://{ IMAGE_RESPONSE_HASH }'}" />
              <button class='btn btn-primary' v-on:click='UploadMetaData()'>Upload meta data</button>
              <br /><span>meta data hash result:</span><input type='text' class='form mb-1' style='width:100%' :value='meta_data_hash_result' />
              <span>meta data hash test 1</span><input type='text' class='form mb-1' style='width:100%' value='bafkreier4i7e7e7reo3iiadarqrj7fgyq3dlad2mdpxfqjtczwqlqecbtq' />
              <br /><span>meta data hash test 2</span><input type='text' class='form mb-1' style='width:100%' value='bafkreifkgidibf4o7foynhs4yi3obzpfq743j37qpugnaqt3g7xcl7iueu' />
          </div>
          <div class='col-sm-3 border p-2'>
              <h4>get file from IPFS</h4>
              <span>IPFS CID </span> <input type='text' class='form mb-1' style='width:70%' v-model='ipfs_hash' />
              <br /><span>File result from IPFS </span> <input type='text' class='form mb-1' style='width:70%' v-model='file_fromIPFS' />
              <br /><img v-if='file_fromIPFS!=""' :src="'data:image/jpeg;base64,'+ file_fromIPFS" />
              <br />
              <button class='btn btn-primary' v-on:click='GetFileFromIPFS()'>Get file from IPFS</button>
          </div>

      </div>

      <div class='row'>

          <div class='col-sm-6 border p-2'>
              <h4>Get all NFTs from a collection</h4>
              <p> All NFT: {{ all_nft}}</p>
              <button class='btn btn-primary' v-on:click='GetAllNft()'>GET ALL NFT</button>
          </div>

          <div class='col-sm-6 border p-2'>
              <h4>Get NFTby hash</h4>
              <span>hash test1 (ETH): 0xef599e167df68defad06ec6a2be6b858b1f2781dcf376635089a18c1b6bb307b</span>
             <br /> <span>hash test2 (ETH): 0x88f69d819ff46ea877d6a9ac67844795f841d3a584036feebb1b65992f66880c</span>
             <br /> <span>hash test3 (MATIC): 0x175d2c0648039b97ec450cab047e83860d47bffa318eddb840c062a8d61b6627</span>
              <br /><span>hash </span> <input type='text' class='form mb-1' style='width:80%' v-model='txid_min' />
              <p> NFT info: {{nft_info}}</p>
              <button class='btn btn-primary mr-2' v-on:click='GetNftByHashETH()'>GET NFT(ETH) by hash</button>
              <button class='btn btn-primary' v-on:click='GetNftByHashMATIC()'>GET NFT(MATIC) by hash</button>
          </div>

      </div>
      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <h4>Get all NFTs that a blockchain address</h4>
              <br />
              <span>blockchain address </span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='address' />
              <p> All NFT: {{ all_nft_from_add}}</p>
              <button class='btn btn-primary mr-2' v-on:click='GetAllNftFromAddETH()'>GET ALL NFT(ETH) from Add</button>
              <button class='btn btn-primary' v-on:click='GetAllNftFromAddMATIC()'>GET ALL NFT(MATIC) from Add</button>
          </div>

          <div class='col-sm-6 border p-2'>
              <h4>Create smart contract ERC721</h4>
              <p> txId smart contract test: {{ tx_id_smart_contract}}</p>
              <br /><span>PrivateKey</span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='private_key' />
              <br /><span>name smart contract (Tên của mã thông báo NFT)</span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='name_smart_contract' />
              <br /><span>symbol smart contract (Biểu tượng của mã thông báo NFT)</span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='symbol_smart_contract' />
              <p> txId smart contract: {{ tx_id_smart_contract}}</p>
              <button class='btn btn-primary' v-on:click='CreateSmartContractERC721()'>Create smart contract</button>
          </div>
      </div>

      <div class='row'>
          <div class='col-sm-6 border p-2'>
              <h4>Create smart contract ERC 20</h4>
              <p> txId smart contract test: {{ tx_id_smart_contract}}</p>
              <br /><span>PrivateKey</span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='private_key' />
              <br /><span>name smart contract (Tên của mã thông báo NFT)</span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='name_smart_contract' />
              <br /><span>symbol smart contract (Biểu tượng của mã thông báo NFT)</span> 
              <input type='text' class='form mb-1' style='width:80%' v-model='symbol_smart_contract' />
              <p> txId smart contract: {{ tx_id_smart_contract}}</p>
              <button class='btn btn-primary' v-on:click='CreateSmartContractERC20()'>Create smart contract</button>
          </div>
      </div>

  </div>
</template>

<script>

export default {
  name: 'HelloWorld',
  props: {
    msg: String
  },
  data: function () {
    return {
      tx_id_smart_contract: '',
      symbol_smart_contract: '',
      name_smart_contract: '',
      type: '?type=testnet',
      msg_err: '',
      public_key: '',
      mnemonic: '',
      address: '',
      address_2: '',
      strErr: '',
      private_key: '',
      current_block_number: '',
      account_balance: '',
      polygon_transation: '',
      block_by_hash: '',
      hash: '',
      vitual_account: '',
      hash_of_transaction: '',
      txid_min: '',
      all_nft: '',
      nft_info: '',
      block_by_addres: '',
      file_upload: '',
      img_name: '',
      ipfs_hash: '',
      meta_data_hash_result: '',
      file_fromIPFS: '',
      all_nft_from_add: ''
      // modelSearchPost: {
      //    reqPagination: {
      //        currentPage: 1,
      //        numberOfRecord: 4,
      //        totalRec: 0,
      //    },
      //    idCategory: 0,
      //    title: '',
      // },
    }
  },

  mounted: function () {
    var self = this
    self.strErr = 'helo long'
    self.public_key = 'xpub6DnS3us5jezXqcJHYKauyPffuXfdHbN6a9k42yHbYMXk7vLsgAAWKsptHBhKzNmZu6QhNfik4T7PBEGVqy6VEU6hXuk1N9hpppZAbtk8hVM'
    self.mnemonic = 'dune toe fork lake small skin tragic polar monkey cannon ice bread excess chef invite meat deputy can brass theme require blanket amused pizza'
    self.address = '0x9c17486cf8d3d194792d781c15735aea13bd5dfd'
    self.address_2 = '0xd0d1fca10dc5e48c4345abde7ae6f512732b1f3f'
    self.private_key = '0x0f6ea296360cf2a470e10bf5616f94d7483067747c94ad41e636308d106dd72a'
  },

  watch: {
    'modelSearchPost.reqPagination.currentPage': {
      handler: function (after, before) {
        if (after !== before) {
          this.errMsg = ''
          this.firstLoadStatus = false
          this.GetListData()
        }
      },
      deep: true
    }
  },
  
  methods: {
          
    CreateSmartContractERC20: function () {
      var self = this
      self.axios.post('https://api.tatum.io/v3/nft/deploy' + self.type,
          {
          'chain': 'MATIC',
          'name': self.name_smart_contract,
          'symbol': self.symbol_smart_contract,
          'fromPrivateKey': self.private_key,
          'cashback': true, // bản quyền cố định
          'provenance': false, // bản quyền %
          'publicMint': true, // true: tất cả có thể đúc, false: chỉ có change tạo bởi private key, hoặc key dc asign mới được đúc
          'fee':{
            'gasLimit': '342440', // min = 374304
            'gasPrice': '2' // --> bao loi không đủ tiền mua gas * giá + giá trị [-32000]'
          }
          },
          {
          'headers': {
            'Content-Type': 'application/json',
            'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
          },
        }
      ).then((response) => {
        console.log('SmartContract address: ' + response.data)
        self.tx_id_smart_contract = response.data.txId
      })
    },
    CreateSmartContractERC721: function () {
      var self = this
      self.axios.post('https://api.tatum.io/v3/nft/deploy' + self.type,
          {
          'chain': 'MATIC',
          'name': self.name_smart_contract,
          'symbol': self.symbol_smart_contract,
          'fromPrivateKey': self.private_key,
          'cashback': true, // bản quyền cố định
          'provenance': false, // bản quyền %
          'publicMint': true, // true: tất cả có thể đúc, false: chỉ có change tạo bởi private key, hoặc key dc asign mới được đúc
          'fee':{
            'gasLimit': '342440',  // min = 374304
            'gasPrice': '2' // --> bao loi không đủ tiền mua gas * giá + giá trị [-32000]'
          }
          },
          {
            'headers': {
                'Content-Type': 'application/json',
                'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
            },

          }
      ).then((response) => {
          console.log('SmartContract address: ' + response.data);
          self.tx_id_smart_contract = response.data.txId;
      })
    },

    GetAllNftFromAddETH: function () {
      var self = this
      self.axios.get('https://api.tatum.io/v3/nft/address/balance/ETH/' + self.address + self.type, {
        'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
      }).then((response) => {
        console.log(response.data)
        self.all_nft_from_add = response.data
      })
    },

    GetAllNftFromAddMATIC: function () {
      var self = this
      self.axios.get('https://api.tatum.io/v3/nft/address/balance/MATIC/' + self.address + self.type, {
        'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
      }).then((response) => {
        console.log(response.data)
        self.all_nft_from_add = response.data
      })
    },

    GetFileFromIPFS: function () {
      var self = this
      self.axios.get('https://api.tatum.io/v3/ipfs/' + self.ipfs_hash + self.type, {
        'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
      }).then((response) => {
        console.log(response.data)
        self.file_fromIPFS = response.data
      })
    },
    UploadMetaData: function () {
      var self = this
      const formData = new FormData()
      formData.append('file', self.file_upload)
      self.axios.post('https://api.tatum.io/v3/ipfs' + self.type,
        formData,
        {
          'headers': {
            'Content-Type': 'multipart/form-data',
            'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
          }
        }
      ).then((response) => {
        console.log('meta data Hash: ' + response.data)
        self.meta_data_hash_result = response.data.ipfsHash
      })
    },
    UploadFileToIPFS: function () {
      var self = this
      const formData = new FormData()
      formData.append('file', self.file_upload)
      self.axios.post('https://api.tatum.io/v3/ipfs' + self.type,
        formData,
        {
          'headers': {
            'Content-Type': 'multipart/form-data',
            'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
          }

        }
        ).then((response) => {
        console.log('ipfs Hash: ' + response.data)
        self.ipfs_hash = response.data.ipfsHash
      })
    },
    changeFile: function (e) {
      var self = this
      self.img_name = e.target.files[0].name
      self.file_upload = e.target.files[0]
    },
    GetNftByHashETH: function () {
      var self = this
      self.axios.get('https://api.tatum.io/v3/nft/transaction/ETH/' + self.txid_min + self.type, {
        'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
      }).then((response) => {
        console.log(response.data)
        self.nft_info = response.data
      })
    },

    GetNftByHashMATIC: function () {
      var self = this
      self.axios.get('https://api.tatum.io/v3/nft/transaction/MATIC/' + self.txid_min + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.nft_info = response.data;
              })
    },

    GetAllNft: function () {
      var self = this
      self.axios.get('https://api.tatum.io/v3/nft/collection/ETH/' + self.address + '?pageSize=10' + self.type, {
        'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
      }).then((response) => {
        console.log(response.data)
        self.all_nft = response.data
      })
    },

    MinNFT: function () {
      var self = this
      self.axios.post('https://api.tatum.io/v3/nft/mint' + self.type,
        {
          'chain': 'MATIC', // ETH
          'to': self.address,
          'url': 'ipfs://' + self.meta_data_hash_result
        },
        {
          'headers': {
            'Content-Type': 'application/json',
            'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
          }

        }
      ).then((response) => {
        console.log('txid_min: ' + response.data)
        self.txid_min = response.data.txId
      })
    },

    SendMaticToAddress: function () {
      var self = this
            self.axios.post('https://api.tatum.io/v3/polygon/transaction' + self.type,
              {
                  'to': self.address,
                  'currency': 'MATIC',
                  'amount': self.account_balance,
                  'fromPrivateKey': self.private_key
              },
              {
                  'headers': {
                      'Content-Type': 'application/json',
                      'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
                  },

              }
              ).then((response) => {
                  console.log('hash_of_transaction: ' + response.data);
                  self.hash_of_transaction = response.data.txId;
              })
          },


          GetVirtualAccounts: function () {
              var self = this
              self.axios.get('https://api.tatum.io/v3/ledger/account/?pageSize=10&offset=1', {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.vitual_account = response.data;
              })
          },


          GetBlockByHash: function () {
              var self = this
              if (self.current_block_number == '') {
                  self.msg_err = 'please get current block number';
                  return;
              }
              self.msg_err = '';
              self.axios.get('https://api.tatum.io/v3/polygon/block/' + self.current_block_number + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.block_by_hash = response.data;
                  self.hash = self.block_by_hash.hash;
              })
          },

          GetPolygonTransactionByAddress: function () {
              var self = this
              self.axios.get('https://api.tatum.io/v3/polygon/account/transaction/' + self.address + '?pageSize=10' + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.block_by_addres = response.data.length;
              })
          },

          GetPolygonTransaction: function () {
              var self = this
              self.axios.get('https://api.tatum.io/v3/polygon/transaction/'+ self.hash + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.polygon_transation = response.data;
              })
          },

          GetPolygonAccountBalance: function () {
              var self = this
              self.axios.get('https://api.tatum.io/v3/polygon/account/balance/' + self.address + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.account_balance = response.data.balance;
              })
          },


          GetCurrentBlockNumber: function () {
              var self = this
              self.axios.get('https://api.tatum.io/v3/polygon/block/current' + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.current_block_number = response.data;
              })
          },

          CreatePublicKey: function () {
              var self = this
              self.axios.get('https://api.tatum.io/v3/polygon/wallet' + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.mnemonic = response.data.mnemonic;
                  self.public_key = response.data.xpub;
              })
          },

          CreatePrivateKey: function () {
              var self = this
              self.axios.post('https://api.tatum.io/v3/polygon/wallet/priv' + self.type,
                  {
                      'index': 0,
                      'mnemonic': self.mnemonic

                  },
                  {
                      'headers': {
                          'Content-Type': 'application/json',
                          'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b'
                      },

                  }
              ).then((response) => {
                  console.log('private key: ' + response.data);
                  self.private_key = response.data.key;
              })
          },

          CreateAcountPolygon: function () {
              var self = this
              var xpub = self.public_key;
              var index = 2 // index between 0 to 2 ^ 31.
              self.axios.get('https://api.tatum.io/v3/polygon/address/' + xpub + '/' + index + self.type, {
                  'headers': { 'x-api-key': '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' }
              }).then((response) => {
                  console.log(response.data);
                  self.address = response.data.address;
              })
              // const ethSDK = TatumEthSDK({ apiKey: '9ca5ab4b-2d00-4d69-b2ce-8b97b1647a0b' });
              // const generatedWallet = await ethSDK.api.ethGenerateWallet('<mnemonic phrase>')
              // console.log(generatedWallet)
              //util.setSessionStore('Search', strSearch).done(function () {
              //    document.getElementById('textSearch').value = strSearch;
              //    self.modelSearchPost.title = strSearch;
              //    self.modelSearchPost.reqPagination.numberOfRecord = self.numberOfRecord;
              //    self.firstLoadStatus = true;
              //    self.GetListData();
              //});
          },
      },

  }
</script>

<!-- Add 'scoped' attribute to limit CSS to this component only -->
<style scoped>
  h3 {
      margin: 40px 0 0;
  }

  ul {
      list-style-type: none;
      padding: 0;
  }

  li {
      display: inline-block;
      margin: 0 10px;
  }

  a {
      color: #42b983;
  }
</style>
