const functions = require('@google-cloud/functions-framework');
const axios = require('axios');

// Definir função HTTP
functions.http('gabsAPI', async (req, res) => {
  const { adress } = req.query; // Obter o código da query string

  try {
    const myRequest = `https://public-api.solscan.io/account/tokens?account=${adress}`;
    const headers = {
      "token": 'YOUR-API-SOLANA-TOKEN',
      "Content-Type": "application/json",
    };

    const response = await axios.get(myRequest, { headers });
    const data = response.data;

    // Filtrar o token desejado pelo endereço
    const filteredTokens = data.filter(token => token.tokenAddress === 'poLisWXnNRwC6oBu1vHiuKQzFjGL4XDSu4g9qjz9qVk');

    // Verificar se o token foi encontrado
    if (filteredTokens.length > 0) {
      // Converter o valor do campo "uiAmount" em um número
      const uiAmount = parseFloat(filteredTokens[0].tokenAmount.uiAmount);

      if (uiAmount >= 0.00000001) {
        const aiAmount = filteredTokens[0].tokenAmount.uiAmount;
        res.json({ message: 'Yes, there is an NFT on this account', uiAmount });
      } else {
        res.json({ message: 'No NFT(polisDAO) found on this account', uiAmount: null });
      }
    } else {
      res.json({ message: 'No NFT found on this account', uiAmount: null });
    }
  } catch (error) {
    console.error(error);
    res.status(500).json({ error: 'An error occurred while processing the request. TRY https://us-central1-rapid-outrider-393000.cloudfunctions.net/RatoAPI?adress=solanaID ' });
  }
});
