<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Chatbot AI</title>
    <style>
        body { font-family: Arial, sans-serif; display: flex; justify-content: center; align-items: center; height: 100vh; background-color: #e3f2fd; margin: 0; }
        .container { width: 450px; background: white; padding: 20px; border-radius: 15px; box-shadow: 0px 4px 15px rgba(0, 0, 0, 0.2); text-align: center; }
        .chatbox { height: 350px; overflow-y: auto; border: 1px solid #ccc; padding: 10px; border-radius: 10px; background: #f9f9f9; text-align: left; }
        .message { margin: 8px 0; padding: 10px; border-radius: 8px; max-width: 80%; display: inline-block; }
        .user { background: #007bff; color: white; align-self: flex-end; float: right; }
        .bot { background: #e0e0e0; color: black; align-self: flex-start; float: left; }
        .input-area { display: flex; margin-top: 10px; }
        input { flex: 1; padding: 10px; border: 1px solid #ccc; border-radius: 5px; font-size: 16px; }
        button { padding: 10px; background: #007bff; color: white; border: none; cursor: pointer; border-radius: 5px; font-size: 16px; }
        button:hover { background: #0056b3; }
    </style>
</head>
<body>
    <div class="container">
        <h2>Chatbot AI</h2>
        <div class="chatbox" id="chatbox"></div>
        <div class="input-area">
            <input type="text" id="userInput" placeholder="Nhập câu trả lời..." onkeypress="handleKeyPress(event)">
            <button onclick="sendMessage()">Gửi</button>
        </div>
    </div>

    <script>
        let step = -1;
        let answers = {};
        let questions = [
            "Bạn thích làm việc với: (1) Con người, (2) Máy móc/Công nghệ, (3) Sáng tạo/Nghệ thuật?",
            "Bạn giỏi nhất trong lĩnh vực nào: (1) Giao tiếp, (2) Phân tích/Số liệu, (3) Thiết kế/Sáng tạo?",
            "Bạn tự mô tả mình là: (1) Hướng ngoại, (2) Hướng nội, (3) Cân bằng?",
            "Bạn thích môi trường làm việc nào: (1) Văn phòng, (2) Tự do, (3) Ngoài trời?",
            "Bạn có quan tâm đến mức lương cao hơn không? (1) Có, (2) Không?"
        ];

        function sendMessage() {
            let input = document.getElementById("userInput").value;
            if (!input) return;

            let chatbox = document.getElementById("chatbox");
            chatbox.innerHTML += `<div class='message user'>Bạn: ${input}</div><div style='clear: both;'></div>`;
            document.getElementById("userInput").value = "";
            chatbox.scrollTop = chatbox.scrollHeight;
            
            if (step === -1) {
                if (input.toLowerCase().includes("định hướng nghề nghiệp")) {
                    step++;
                    setTimeout(() => botReply(questions[step]), 500);
                } else {
                    setTimeout(() => botReply("Xin lỗi, tôi chỉ có thể giúp bạn định hướng nghề nghiệp!"), 500);
                }
            } else if (step < questions.length - 1) {
                answers[step] = input;
                step++;
                setTimeout(() => botReply(questions[step]), 500);
            } else {
                answers[step] = input;
                setTimeout(() => suggestCareer(), 500);
            }
        }

        function botReply(message) {
            let chatbox = document.getElementById("chatbox");
            chatbox.innerHTML += `<div class='message bot'>Bot: ${message}</div><div style='clear: both;'></div>`;
            chatbox.scrollTop = chatbox.scrollHeight;
        }

        function suggestCareer() {
            let career = "Bạn có thể phù hợp với nhiều ngành nghề khác nhau! Hãy thử khám phá thêm.";
            if (answers[0] == "1" && answers[1] == "1") career = "Nhà tâm lý học, Chuyên gia nhân sự, Giáo viên";
            else if (answers[0] == "1" && answers[1] == "2") career = "Nhà kinh tế học, Nhà phân tích dữ liệu, Quản lý kinh doanh";
            else if (answers[0] == "2" && answers[1] == "2") career = "Kỹ sư phần mềm, Kỹ sư cơ khí, Chuyên gia AI";
            else if (answers[0] == "2" && answers[1] == "3") career = "Nhà phát triển game, Kiến trúc sư công nghệ, Nhà thiết kế UI/UX";
            else if (answers[0] == "3" && answers[1] == "3") career = "Họa sĩ, Nhà văn, Đạo diễn phim";
            
            botReply(`Dựa trên câu trả lời của bạn, nghề nghiệp phù hợp là: ${career}`);
        }

        function handleKeyPress(event) {
            if (event.key === "Enter") {
                sendMessage();
            }
        }

        window.onload = function() {
            botReply("Chào bạn! Bạn cần tôi giúp gì?");
        };
    </script>
</body>
</html>
