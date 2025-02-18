<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot - Google AI</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        body {
            font-family: Arial, sans-serif;
            background-color: white;
            display: flex;
            flex-direction: column;
            align-items: center;
            height: 100vh;
        }
        .navbar {
            width: 100%;
            background-color: #333;
            overflow: hidden;
            display: flex;
            justify-content: center;
            padding: 10px 0;
        }
        .navbar a {
            color: white;
            text-decoration: none;
            padding: 14px 40px;
            display: inline-block;
	    font-size:20px;
        }
        .navbar a:hover {
	    text-decoration:underline;
        }
        .chat-container {
            width: 400px;
            height: 600px;
            background-color: white;
            border-radius: 30px;
            box-shadow: 0 0 100px rgba(0, 0, 0, 0.1);
            display: flex;
            flex-direction: column;
            padding: 10px;
            display: none;
            resize: both;
            overflow: auto;
            margin-top: 20px;
        }
        .bot-message, .user-message {
            margin: 10px 0;
            padding: 8px;
            border-radius: 15px;
            width: fit-content;
            max-width: 70%;
        }
        .bot-message { background-color: #d1f7d1; }
        .user-message { background-color: #d1d1f7; margin-left: auto; }
        .user-input {
            width: 90%;
            padding: 10px;
            border-radius: 15px;
            border: 2px solid gray;
	    position:sticky;
	    top:100%;
        }
        #bot {
            margin-top: 65px;
            border-radius: 50px;
            border: none;
            color: white;
            background-color: black;
            width: 50px;
            height: 30px;
            cursor: pointer;
	    position:absolute;
        }
        .chat-box {
            flex-grow: 1;
            overflow-y: auto;
            margin-bottom: 10px;
            padding: 10px;
            background-color: white;
            border-radius: 20px;
            max-height: 485px;
        }
        .copy-btn {
            background: black;
            color: white;
            border: none;
            padding: 4px 6px;
            border-radius: 50px;
            font-size: 12px;
            cursor: pointer;
            display: none;
	    position: sticky;
            left: 0px;
            top: 0px;
        }
        .copied {
            background: black !important;
        }
        .bot-message:hover .copy-btn,.user-message:hover .copy-btn {
            display: block;
        }
	.bot-message a {color: black !important;text-decoration:none;}
	.bot-message:hover .copy-btn{position:sticky;}
	#wee{position:sticky;font-size:8px;}
	.gray{cursor:pointer;}
	.gray:hover{filter: grayscale(0%) !important;transition: filter 0.5s ease;}
	        .loading-dots {
            display: flex;
            justify-content: center;
            align-items: center;
        }
        .dot {
            width: 8px;
            height: 8px;
            background-color: black;
            border-radius: 50%;
            margin: 0 2px;
            animation: bounce 1.4s infinite ease-in-out both;
        }
        .dot:nth-child(1) { animation-delay: -0.32s; }
        .dot:nth-child(2) { animation-delay: -0.16s; }
        .dot:nth-child(3) { animation-delay: 0; }

        @keyframes bounce {
            0%, 80%, 100% { transform: scale(0); opacity: 0.5; }
            50% { transform: scale(1); opacity: 1; }
        }
	#menu{right:96%;position:fixed;top:1%;color:white;background-color: #333;font-size: 20px;cursor:pointer;text-decoration:none;left:1px;}
	#history {
            height: 100%;
            width: 0;
            position: fixed;
            z-index: 1;
            top: 0;
            left: 0;
            background-color: #333;
            overflow-x: hidden;
            transition: 0.5s;
            padding-top: 60px;
        }
        #history .close-btn {
            position: absolute;
            top: 0;
            right: 25px;
            font-size: 36px;
            margin-left: 50px;
        }
	.close-btn{text-decoration:none;color:white;margin-top:-5px;left:-50px;width:1px;}
	.close-btn:hover{color:red;}
        #history ul {
            padding: 20px;
            list-style: none;
	    color:white;
        }
        #history ul li {
            padding: 8px;
            cursor: pointer;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        #history ul li:hover { background-color: #555; }
	#history h2{color:white;}
        .delete-btn {
            background: transparent;
            border: none;
            color: white;
            font-size: 16px;
            cursor: pointer;
        }

	#oo{border:none;border-bottom: 3px solid black;outline:none;}
	#oop{border:none;cursor:pointer;font-size:15px;}
	#email{border:none;outline:none;}
	#subject{border:none;outline:none;border-bottom: 3px solid black;}
	#message{border:none;outline:none;}
	        .typing-container {
            font-family: 'Courier New', monospace;
            font-size: 24px;
            display: inline-block;
            position: relative;
        }
	#message:hover {
            border: none;
            border-bottom: 3px solid black;
        }
	#phone{border:none;outline:none;border-bottom: 3px solid black;}
	#messagesms{border:none;outline:none;}
	        .typing-container {
            font-family: 'Courier New', monospace;
            font-size: 24px;
            display: inline-block;
            position: relative;
        }
	#messagesms:hover {
            border: none;
            border-bottom: 3px solid black;
        }
	#send{border:none;cursor:pointer;}
        .typing-container::after {
            content: '🍃';
            position: absolute;
            right: -2px;
            animation: blink 2s infinite;
        }

        @keyframes blink {
            0% {
                opacity: 1;
            }
            50% {
                opacity: 0;
            }
            100% {
                opacity: 1;
            }
        }
	.form {display: none; /* Initially hidden */
            width: 65%;
            height: 70%;
            background-color: white;
            padding: 20px;
	    margin-left: -54%;
	    margin-top: 5%;
            border-radius: 10px;
            box-shadow: 0 0 5000px rgba(0, 0, 0, 0.2);
	    position:absolute;
	}
    </style>
</head>
<body>
    <div class="navbar">
<a id="menu" onclick="openNav()">☰</a>
        <a href="#">Home</a>
        <a href="#" id="email">Email</a>
        <a href="#" id="sms">SMS</a>
        <a href="#" id="otp">OTP</a>
        <a href="#">Contact</a>
<div id="container">
  <!-- Form 1 -->
  <div id="eml" class="form">
<center>
<h2>Send Email Notification</h2><br><br>
<label><b>EMAIL :<b></label>
<input type="email" id="email" placeholder="TO"><br><br><br>
<label>SUBJECT :</label>
<input type="text" id="subject" placeholder=""><br><br><br>
<div class="typing-container"></div>
<label>MESSAGE</label><br><br>
<textarea id="message" placeholder=""></textarea><br><br><br>
<button type="submit" id="send" onclick="sendMail()"><h3>SEND</h3></button>
</center>
    <!-- Form 1 content here -->
  </div>

  <!-- Form 2 -->
  <div id="sm" class="form" style="display: none;">
<center>
<h2>Send SMS Notification</h2><br><br>
<label><b>MOBILE NO :<b></label>
<input type="text" id="phone" value="+91 " required><br><br><br>
<div class="typing-container"></div>
<label>MESSAGE</label><br><br>
<textarea id="messagesms" placeholder=""></textarea><br><br><br>
<button type="button" id="send" onclick="sendSMS()"><h3>SEND</h3></button>
</center>
    <!-- Form 2 content here -->
  </div>

  <!-- Form 3 -->
  <div id="ot" class="form" style="display: none;">
<center>
<h2>Send OTP</h2><br><br>
<h1 id="otpDisplay"></h1><br><br>
<h2 id="timerDisplay">Enter OTP in 30 seconds...</h2><br><br><br>
<button onclick="refreshOTP()" id="oop"><h3>⭮</h3></button>
<div class="typing-container"></div>
<input type="text" id="oo" id="timerDisplay" placeholder="Enter OTP">
<button type="button" id="send" onclick="check()"><h3>CHECK</h3></button><br><br><br>
<h1 id="ooo"></h1>
</center>
    <!-- Form 3 content here -->
  </div>
    </div>

    <div class="chat-container">
        <h2>Chatbot</h2>
        <div class="chat-box" id="chat-box">
            <div class="bot-message">
                <button class="copy-btn" onclick="copyToClipboard(this)">Copy</button>
		<center>Hello! How can I assist you today?</center>
            </div>
        </div>
        <input type="text" autofocus id="user-input" class="user-input" placeholder="Type a message...">
    </div>
	<button id="bot" onclick="fun()">BOT</button>
	<p id="wee"></p>
<div id="history">
<a href="javascript:void(0)" class="close-btn" onclick="closeNav()">&times;</a>
<center>
	<h2>Search History</h2>
        <ul id="history-list"></ul>
</center>
</div>

</div>
</div>

<script type="text/javascript" src="https://cdn.jsdelivr.net/npm/@emailjs/browser@4/dist/email.min.js"></script>
<script>
        function toggleChat() {
            let chatContainer = document.getElementById("chat-container");
            chatContainer.style.display = (chatContainer.style.display === "none" || chatContainer.style.display === "") ? "block" : "none";
        }
        document.getElementById('user-input').addEventListener('keydown', function(event) {
            if (event.key === 'Enter') {
                event.preventDefault(); // Prevent form submission behavior
                sendMessage();
		saveSearch();
            }
        });

async function sendMessage() {
    const userInput = document.getElementById('user-input').value;
    if (userInput.trim() !== '') {
        displayMessage(userInput, "user-message");
        document.getElementById('user-input').value = '';

        // Check if the query is about images
        const imageKeywords = ["image", "images", "photo", "photos", "img", "imgs", "pic", "pics", "picture", "pictures"];
        if (imageKeywords.some(keyword => userInput.toLowerCase().includes(keyword))) {
            await searchImages(userInput);
            return;  // Skip text response to prevent ASCII art
        }

        // Show loading animation
        const loadingDiv = document.createElement('div');
        loadingDiv.classList.add("bot-message");
        loadingDiv.id = "loading-message";
        loadingDiv.innerHTML = `<div class="loading-dots"><div class="dot"></div><div class="dot"></div><div class="dot"></div></div>`;
        document.getElementById('chat-box').appendChild(loadingDiv);
        document.getElementById('chat-box').scrollTop = document.getElementById('chat-box').scrollHeight;

        try {
            let botResponse = await getBotResponse(userInput);
            loadingDiv.remove();

            // Format response text
            botResponse = botResponse.replace(/\*/g, '')
                .replace(/\:/g, ':<br>&nbsp;&nbsp;&nbsp;&nbsp;')
                .replace(/\?/g, '?<br>')
                .replace(/(.*?):/g, '<b>$1</b>:')
                .replace(/(.*?):/g, '<br><b>$1</b>:')
                .replace(/\s. s/g, '.<br><br>')
                .replace(/\]/g, ']<br>')
                .replace(/(?<!\([^\)]*)\.(?![^\(]*\))(?!<b>)/g, '.<br><br>');

            displayMessage(botResponse, "bot-message", true);
        } catch (error) {
            console.error('Error fetching response:', error);
            displayMessage("Sorry, I'm having trouble responding right now.", "bot-message", false);
        }
    }
}

        function displayMessage(message, type, isHTML = false) {
            const chatBox = document.getElementById('chat-box');

            const messageDiv = document.createElement('div');
            messageDiv.classList.add(type);

            if (isHTML) {
                messageDiv.innerHTML = message;
            } else {
                messageDiv.textContent = message;
            }

            const copyButton = document.createElement('button');
            copyButton.classList.add('copy-btn');
            copyButton.textContent = "Copy";
            copyButton.onclick = function () {
                copyToClipboard(this);
            };

            messageDiv.appendChild(copyButton);
            chatBox.appendChild(messageDiv);

            chatBox.scrollTop = chatBox.scrollHeight;
        }

        async function getBotResponse(userMessage) {
            const apiKey = "AIzaSyAqQlGzfpFCW-b8CTsut0dr5ID7kIgqQEo";
            const apiUrl = `https://generativelanguage.googleapis.com/v1/models/gemini-pro:generateContent?key=${apiKey}`;

            try {
                const response = await fetch(apiUrl, {
                    method: "POST",
                    headers: { "Content-Type": "application/json" },
                    body: JSON.stringify({ contents: [{ role: "user", parts: [{ text: userMessage }] }] })
                });

                const data = await response.json();
                return data.candidates[0].content.parts[0].text || "I'm not sure how to respond.";
            } catch (error) {
                console.error('Error fetching AI response:', error);
                return "Sorry, I'm having trouble responding right now.";
            }
        }

        function copyToClipboard(button) {
            const messageText = button.parentElement.textContent.replace("Copy", "").trim();
            navigator.clipboard.writeText(messageText).then(() => {
                button.textContent = "Done";
                button.classList.add('copied');
                setTimeout(() => {
                    button.textContent = "Copy";
                    button.classList.remove('copied');
                }, 2000);
            });
        }

        let clickCount = 0;
        let timeout;

        document.querySelector("#bot").addEventListener("click", function(){
            let chatContainer = document.querySelector(".chat-container");

            if (chatContainer.style.display === "none" || chatContainer.style.display === "") {
                chatContainer.style.display = "block";  
            } else {
                chatContainer.style.display = "none";  
            }

            clickCount++;

            if (clickCount === 3) {
                location.reload();
            }

            clearTimeout(timeout);
            timeout = setTimeout(() => {
                clickCount = 0;
            }, 2500); 
        });

        function fun() {
            document.getElementById("wee").innerHTML = "";
        }

        const apiKey = 'zPDGq4jbaKY9X49K4fRpkEYD4EjBs3l7VPNZCFwGRhA';

        async function searchImages(query) {
            const url = `https://api.unsplash.com/photos/random?query=${query}&client_id=${apiKey}&count=5`;

            try {
                const response = await fetch(url);
                const data = await response.json();

                const chatBox = document.getElementById('chat-box');

                if (data.length > 0) {
                    // Display images as part of bot response
                    const imageDiv = document.createElement('div');
                    imageDiv.classList.add('bot-message');
                    imageDiv.innerHTML = `<b>Here are some images for you:</b><br>`;

                    data.forEach(image => {
                        const imgElement = document.createElement('img');
                        imgElement.src = image.urls.small;
                        imgElement.alt = image.alt_description || 'Unsplash Image';
                        imgElement.style.width = '100%';
			imgElement.style.filter = 'grayscale(100%)';
			imgElement.classList.add('gray');
                        imgElement.style.margin = '10px 0';
                        imageDiv.appendChild(imgElement);
                    });

                    chatBox.appendChild(imageDiv);
                } else {
                    displayMessage("Sorry, I couldn't find any images for that query.", "bot-message", false);
                }

                chatBox.scrollTop = chatBox.scrollHeight;

            } catch (error) {
                console.error('Error fetching images:', error);
                displayMessage("Sorry, I'm having trouble fetching images right now.", "bot-message", false);
            }
        }

 document.addEventListener("DOMContentLoaded", function () {
        showSearchHistory();
    });
        function saveSearch() {
            const userInput = document.getElementById('user-input').value.trim();
            if (userInput) {
                let searchHistory = JSON.parse(localStorage.getItem("searchHistory")) || [];
                searchHistory.push(userInput);
                localStorage.setItem("searchHistory", JSON.stringify(searchHistory));
                showSearchHistory();
                document.getElementById('user-input').value = ''; // Clear input after saving
            }
        }

function showSearchHistory() {
            const historyList = document.getElementById("history-list");
            historyList.innerHTML = "";
            let searchHistory = JSON.parse(localStorage.getItem("searchHistory")) || [];

searchHistory.forEach((search, index) => {
                const listItem = document.createElement("li");
                listItem.textContent = search;
                listItem.style.cursor = "pointer";

listItem.addEventListener("click", function () {
                    document.getElementById('user-input').value = search;
                    sendMessage();
                });

const deleteButton = document.createElement("button");
                deleteButton.textContent = "🗙";
                deleteButton.classList.add("delete-btn");
                deleteButton.onclick = function (event) {
                    event.stopPropagation(); 
                    searchHistory.splice(index, 1);
                    localStorage.setItem("searchHistory", JSON.stringify(searchHistory));
                    showSearchHistory();
                };

listItem.appendChild(deleteButton);
                historyList.appendChild(listItem);
            });
        }
function openNav() {
            document.getElementById("history").style.width = "250px";
        }

        // Close the side navigation
function closeNav() {
            document.getElementById("history").style.width = "0";
        }
document.getElementById('email').addEventListener('click', function(event) {
            event.preventDefault();
            toggleForm('eml', 'email', 'Email');
        });

document.getElementById('sms').addEventListener('click', function(event) {
            event.preventDefault();
            toggleForm('sm', 'sms', 'SMS');
        });

document.getElementById('otp').addEventListener('click', function(event) {
            event.preventDefault();
            toggleForm('ot', 'otp', 'OTP');
        });

function toggleForm(formId, linkId, originalText) {
            const selectedForm = document.getElementById(formId);
            const link = document.getElementById(linkId);

 if (!selectedForm) return;

            // Check if the form is already visible
const isVisible = selectedForm.style.display === 'block';

            // Hide all forms
document.querySelectorAll('.form').forEach(form => {
                form.style.display = 'none';
            });

            // Reset all links to original text
document.getElementById('email').innerText = "Email";
            document.getElementById('sms').innerText = "SMS";
            document.getElementById('otp').innerText = "OTP";
            // Toggle visibility of the selected form
            if (!isVisible) {
                selectedForm.style.display = 'block';
                link.innerText = "Close";
            }
        }

 function sendSMS() {
            const phone = document.getElementById("phone").value;
            const message = document.getElementById("messagesms").value;
            if (!phone || !message) {
                alert("Please enter a phone number and message.");
                return;
            }

            // Twilio API Details
const accountSid = "AC73df8443639ceebc7522ea53e25b150b"; // Replace with your Twilio SID
            const authToken = "205e59d672c3afbffd7fa7c44b5bf183";   // Replace with your Twilio Auth Token
            const twilioNumber = "+18315083122";     // Twilio phone number

            // Twilio API URL
const url = `https://api.twilio.com/2010-04-01/Accounts/${accountSid}/Messages.json`;

            // Data to Send
const data = new URLSearchParams();
            data.append("To", phone);
            data.append("From", twilioNumber);
            data.append("Body", message);

            // Fetch API to send SMS
fetch(url, {
                method: "POST",
                headers: {
                    "Authorization": "Basic " + btoa(accountSid + ":" + authToken),
                    "Content-Type": "application/x-www-form-urlencoded"
                },
                body: data
            })
            .then(response => response.json())
            .then(result => {
                console.log("SMS Sent!", result);
                alert("SMS Sent Successfully!");
            })
            .catch(error => {
                console.error("SMS Sending Failed!", error);
                alert("SMS Sending Failed!");
            });
        }
let otp = generateOTP();
let timer; 
let countdown = 30;

function generateOTP() {   
    let digits = '0123456789'; 
    let OTP = ''; 
    let len = digits.length; 
    for (let i = 0; i < 4; i++) { 
        OTP += digits[Math.floor(Math.random() * len)]; 
    } 
    return OTP; 
}

function refreshOTP() {
    otp = generateOTP();
    document.getElementById("otpDisplay").textContent = otp;
    resetTimer(); 
}

function check() {
    var a = document.getElementById("oo").value; 
    if (a === otp) {
        document.getElementById("ooo").innerHTML = "Correct!";  
    } else {
        document.getElementById("ooo").innerHTML = "Incorrect!";
    }
}

function startTimer() {
    timer = setInterval(() => {
        if (countdown > 0) {
            document.getElementById("timerDisplay").textContent = `Enter OTP in ${countdown} seconds...`;
            countdown--;
        } else {
            clearInterval(timer); 
            refreshOTP(); 
        }
    }, 1000);
}

function resetTimer() {
    clearInterval(timer); 
    countdown = 30; 
    startTimer(); 
}

document.getElementById("otpDisplay").textContent = otp;
startTimer();

function sendMail() {
            let params = {
                email: document.getElementById("email").value,
                subject: document.getElementById("subject").value,
                message: document.getElementById("message").value
            };

 emailjs.send("service_b27jf7s", "template_x8q13nb", params)
                .then(function(response) {
                    alert("Email Sent Successfully!");
                    console.log(response);
                })
                .catch(function(error) {
                    alert("Email Sending Failed");
                    console.error(error);
                });
        }
        emailjs.init("UQdOZ2RCsxdSixeg0");
</script>
    </script>
</body>
</html>
