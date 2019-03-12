<template>
  <div id='app'>
    <h1>Vue + Ethereum = 😍</h1>

    <vth-status>
      <template
        slot-scope='{
          loading,
          enabled,
          available,
          account,
          networkVersion
        }'
      >
        <p v-if='loading'>Waiting on you, Web3...</p>

        <template v-else-if='enabled'>
          <h4>Your Web3 Details:</h4>

          <p>Address: <br />{{ account }}</p>
          <p>Blockie: <br /><vth-blockie :string='account' /></p>
          <p>Network Version: <br />{{ networkVersion }}</p>

          <h4>Call a smart contract's method:</h4>
          <vth-contract v-bind:address='address' v-bind:abi='abi'>
            <button
              slot-scope='contract'
            >
              Click me
            </button>
          </vth-contract>
          <p>
            <small
              >(Note: this contract is not deployed so it won't work.)</small
            >
          </p>

          <h4>Details for a transaction on this network:</h4>
          <vth-tx :tx='txByNetwork[networkVersion]'>
            <pre slot-scope='tx'>{{ tx }}</pre>
          </vth-tx>
        </template>

        <button v-else-if='available' @click='getWeb3'>Enable Web3</button>

        <p v-else>
          Looks like you don't have Web3. Maybe you should install
          <a href='https://metamask.io/'>MetaMask</a> 😉
        </p>
      </template>
    </vth-status>
  </div>
</template>

<script>
// Provide the contract address and abi from somewhere
import { _address, _abi } from './exampleContract';

export default {
  data: () => ({
    address: _address,
    abi: _abi,
    txByNetwork: {
      1: '0x07cd699cef633044e84537df775254abbbabb59a6aa925e994c8e8a757adc1c0',
      3: '0xd707e13561f21093214b90c2f8fa41d7b13b64579915aa29fe884a2d148353f6',
      42: '0xbe413faaaf591fce714d0fea808516953b259c8c1e9eddf08d31e4e0e00bfeed',
      4: '0x819594f247d17984510bc5e5bf792acb717f888c2133593bb4a86ca88e3ae993',
    },
  }),

  methods: {
    // async callMethod(method, ...params) {
    //   // const results = await method(...params);
    //   // // Do something with results
    // },
  },
};
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

h4 {
  margin-top: 100px;
}

pre {
  max-width: 100%;
  border-radius: 5px;
  padding: 15px;
  word-wrap: break-word;
  white-space: pre-wrap;
  text-align: left;
  background: #eee;
}
</style>
