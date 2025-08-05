<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Kiếm Tiền Điểm Danh TikTok</title>
  <link rel="stylesheet" href="style.css">
</head>
<body>
  <div class="game-container">
    <h1>Kiếm Tiền Điểm Danh TikTok</h1> <!-- Changed title here -->
    <p id="balance">Số dư: 0 VND</p>
    <button id="clickButton">Bấm để cộng 5,000 vào số dư</button>
    <br><br>

    <!-- Withdraw Section -->
    <h2>Rút tiền</h2>
    <input type="text" id="bankName" placeholder="Tên ngân hàng" />
    <input type="text" id="accountNumber" placeholder="Số tài khoản" />
    <input type="text" id="accountHolder" placeholder="Tên người nhận" />
    <input type="number" id="withdrawAmount" placeholder="Nhập số tiền cần rút" />
    <button id="withdrawButton">Rút tiền</button>

    <p id="withdrawalStatus"></p>
  </div>

  <script src="script.js"></script>
</body>
</html>
body {
  font-family: Arial, sans-serif;
  background: linear-gradient(135deg, #ff2a68, #00c6ff); /* TikTok inspired gradient background */
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100vh;
  margin: 0;
  color: white; /* Set text color to white for contrast */
}

.game-container {
  text-align: center;
  padding: 30px;
  background-color: rgba(0, 0, 0, 0.6); /* Semi-transparent black background */
  border-radius: 15px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.2);
  max-width: 400px;
  width: 100%;
}

h1 {
  font-size: 2.5rem;
  color: #ff5d57; /* TikTok's accent red color */
  text-transform: uppercase;
  margin-bottom: 20px;
}

button {
  background-color: #ff5d57; /* TikTok-inspired red */
  color: white;
  padding: 12px 25px;
  border: none;
  border-radius: 8px;
  cursor: pointer;
  font-size: 18px;
  margin-top: 15px;
  transition: background-color 0.3s ease;
}

button:hover {
  background-color: #ff2a68; /* Darker shade on hover */
}

input[type="text"],
input[type="number"] {
  padding: 12px;
  font-size: 16px;
  border: none;
  border-radius: 8px;
  margin-top: 10px;
  width: 100%;
  margin-bottom: 15px;
  background-color: #1c1c1c;
  color: white;
}

h2 {
  font-size: 1.8rem;
  margin-top: 30px;
  color: #00c6ff; /* TikTok-inspired blue */
}

p {
  font-size: 1.2rem;
  margin-top: 10px;
}

#withdrawalStatus {
  font-size: 1.1rem;
  margin-top: 20px;
}
// Declare a variable to store the balance
let balance = 0;

// Get the balance element, buttons, and input fields
const balanceElement = document.getElementById("balance");
const clickButton = document.getElementById("clickButton");
const withdrawButton = document.getElementById("withdrawButton");
const withdrawAmount = document.getElementById("withdrawAmount");
const bankName = document.getElementById("bankName");
const accountNumber = document.getElementById("accountNumber");
const accountHolder = document.getElementById("accountHolder");
const withdrawalStatus = document.getElementById("withdrawalStatus");

// Function to format the balance with commas and VND
function formatCurrency(amount) {
  return amount.toLocaleString('vi-VN') + ' VND'; // Using 'vi-VN' to format for Vietnamese currency
}

// Update the displayed balance
function updateBalance() {
  balanceElement.textContent = `Số dư: ${formatCurrency(balance)}`;
}

// Event listener for the "Add Balance" button
clickButton.addEventListener("click", function() {
  // Add 5,000 to the balance
  balance += 5000;
  // Update the balance display
  updateBalance();
});

// Event listener for the "Withdraw" button
withdrawButton.addEventListener("click", function() {
  // Show initial processing message
  withdrawalStatus.textContent = "Đang xử lý, vui lòng chờ 1 đến 2 tiếng...";
  withdrawalStatus.style.color = "orange";
  
  // Get the withdrawal amount, bank name, account number, and account holder's name
  const amountToWithdraw = parseInt(withdrawAmount.value);
  const bank = bankName.value.trim();
  const account = accountNumber.value.trim();
  const holder = accountHolder.value.trim();

  // Simulate a delay to show the processing message
  setTimeout(function() {
    const minWithdrawalAmount = 200000; // Minimum withdrawal amount: 200,000 VND
    const maxWithdrawalAmount = 10000000000; // Maximum withdrawal amount: 10 billion VND

    // Check if the withdrawal amount is valid and meets the minimum and maximum withdrawal amounts
    if (isNaN(amountToWithdraw) || amountToWithdraw <= 0) {
      withdrawalStatus.textContent = "Vui lòng nhập số tiền hợp lệ.";
      withdrawalStatus.style.color = "red";
      return;
    }

    if (amountToWithdraw < minWithdrawalAmount) {
      withdrawalStatus.textContent = `Số tiền rút tối thiểu là ${formatCurrency(minWithdrawalAmount)}.`;
      withdrawalStatus.style.color = "red";
      return;
    }

    if (amountToWithdraw > maxWithdrawalAmount) {
      withdrawalStatus.textContent = `Số tiền rút tối đa là ${formatCurrency(maxWithdrawalAmount)}.`;
      withdrawalStatus.style.color = "red";
      return;
    }

    if (!bank || !account || !holder) {
      withdrawalStatus.textContent = "Vui lòng nhập đầy đủ thông tin ngân hàng, tài khoản và người nhận.";
      withdrawalStatus.style.color = "red";
      return;
    }

    if (amountToWithdraw > balance) {
      withdrawalStatus.textContent = "Số dư không đủ để rút.";
      withdrawalStatus.style.color = "red";
    } else {
      // Subtract the withdrawal amount from the balance
      balance -= amountToWithdraw;
      // Update the balance display
      updateBalance();
      
      // Display a success message
      withdrawalStatus.textContent = `Rút ${formatCurrency(amountToWithdraw)} thành công từ tài khoản ${account} tại ngân hàng ${bank}, người nhận: ${holder}.`;
      withdrawalStatus.style.color = "green";
    }

    // Clear the input fields
    withdrawAmount.value = "";
    bankName.value = "";
    accountNumber.value = "";
    accountHolder.value = "";
  }, 2000); // Simulate 2 seconds delay to show the processing message
});
