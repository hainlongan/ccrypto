# ccrypto
ccrypto
node-cryptometh

node-cryptometh is a simple wrapper for the cryptometh client's JSON-RPC API.
Install

npm install node-cryptometh
Examples
Create client

var client = new cryptometh.Client({
  host: 'localhost',
  port: 15715,
  user: 'username',
  pass: 'password'
});

Get balance across all accounts with minimum confirmations of 6

client.getBalance('*', 6, function(err, balance) {
  if (err) return console.log(err);
  console.log('Balance:', balance);
});

Getting the balance directly using cmd

client.cmd('getbalance', '*', 6, function(err, balance){
  if (err) return console.log(err);
  console.log('Balance:', balance);
});

Batch multiple RPC calls into single HTTP request

var batch = [];
for (var i = 0; i < 10; ++i) {
  batch.push({
    method: 'getnewaddress',
    params: ['myaccount']
  });
}
client.cmd(batch, function(err, address) {
  if (err) return console.log(err);
  console.log('Address:', address);
});

SSL

See Enabling SSL on original client.

If you're using this to connect to cryptomethd across a network it is highly recommended to enable ssl, otherwise an attacker may intercept your RPC credentials resulting in theft of your cryptomeths.

When enabling ssl by setting the configuration option to true, the sslStrict option (verifies the server certificate) will also be enabled by default. It is highly recommended to specify the sslCa as well, even if your cryptomethd has a certificate signed by an actual CA, to ensure you are connecting to your own cryptomethd.

var client = new cryptometh.Client({
  host: 'localhost',
  port: 15715,
  user: 'username',
  pass: 'password',
  ssl: true,
  sslStrict: true,
  sslCa: fs.readFileSync(__dirname + '/server.cert')
});

If your using a self signed certificate generated with something like

openssl x509 -req -days 365 -in server.cert -signkey server.key -out server.cert

then sslStrict should be set to false because by defult node wont work with untrusted certificates.
