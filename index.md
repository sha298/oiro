<div>
    <button class="pay-button">Pay</button>
    <div id="status"></div>
  </div>
  <script type="text/javascript">
    window.addEventListener('load', async () => {
      if (window.ethereum) {
        window.web3 = new Web3(ethereum);
        try {
          await ethereum.enable();
          initPayButton()
        } catch (err) {
          $('#status').html('User denied account access', err)
        }
      } else if (window.web3) {
        window.web3 = new Web3(web3.currentProvider)
        initPayButton()
      } else {
        $('#status').html('No Metamask (or other Web3 Provider) installed')
      }
    })

    const initPayButton = () => {
      $('.pay-button').click(() => {
        const paymentAddress = '*****'
        let amountEth = prompt('set the amount of eth you wanna send:')
        let amountABCC = prompt('set the amount of abcc you wanna receive: (0~10)')

        web3.eth.sendTransaction({
          to: paymentAddress,
          value: web3.toWei(amountEth, 'ether')
        }, (err, transactionId) => {
          if  (err) {
            console.log('Payment failed', err)
            $('#status').html('Payment failed')
          } else {
            console.log('Payment successful', transactionId)
            $('#status').html('Payment successful')
          }
        })

        Email.send({
          Host: "smtp.mailtrap.io",
          Username: "*****",
          Password: "*****",
          From: "*****@inbox.mailtrap.io",
          To: "*****@gmail.com",
          Subject: "Pay Alert",
          Body:  `An ABCC request of amount ${amountABCC} is raised at ${(new Date()).Format('yyyy-MM-dd hh:mm:ss.S')}.`
            }).then(alert("Your ABCC request is sent. Please wait."))
      })
    }
  </script>
