<script setup>
import { ref } from 'vue'
import { connect, disconnect } from "starknetkit"

const walletName = ref(null);
const walletAddress = ref(null);

async function handleConnect() {
  const { wallet } = await connect({
    argentMobileOptions: {
      dappName: "Dapp name",
      chainId: 'SN_MAIN',
      icons: []
    }
  })

  walletName.value = wallet.name
  
  if (!wallet.isConnected) {
    await wallet.enable();
  }

  walletAddress.value = wallet.account.address
}

async function handleDisconnect() {
  await disconnect()
  walletName.value = null
  walletAddress.value = null
}
</script>

<template>
  <div>
    <button v-if="!walletName" @click="handleConnect">Connect</button>
    <template v-if="walletName">
      Connected to {{ walletName }} {{walletAddress}}
      <button @click="handleDisconnect">Disconnect</button>
    </template>
  </div>
</template>
