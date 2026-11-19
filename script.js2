
const chatBox = document.getElementById("chatBox");
const userInput = document.getElementById("userInput");
const sendBtn = document.getElementById("sendBtn");
const clearBtn = document.getElementById("clearBtn");

// FAQ dataset
const faqs = [
    {
        questions: ["when are the exams", "exam timetable", "examination schedule"],
        answer: "You can find examination schedules on your college notice board or official website."
    },
    {
        questions: ["what are college timings", "college opening time", "college hours"],
        answer: "Please check your college timetable for official college timings."
    },
    {
        questions: ["how much are the fees", "college fee payment", "fee details"],
        answer: "For fee details, please contact your college office."
    },
    {
        questions: ["where is the library", "library books", "college library"],
        answer: "You can visit the college library to borrow books and study materials."
    },
    {
        questions: ["what subjects are taught", "list of subjects", "course details"],
        answer: "Please refer to your department syllabus for the list of subjects."
    },
    {
        questions: ["when are holidays", "college holidays", "holiday list"],
        answer: "Holiday information is usually shared through college announcements."
    },
    {
        questions: ["how to get admission", "admission process", "college admission"],
        answer: "Contact the college admission office for admission information."
    },
    {
        questions: ["hello", "hi", "hey"],
        answer: "Hello! Welcome to College FAQ Assistant. How can I help you?"
    },
    {
        questions: ["thank you", "thanks"],
        answer: "You're welcome! Feel free to ask another question."
    }
];

// Text preprocessing
function preprocess(text) {
    const stopWords = ["is", "are", "the", "a", "an", "to", "of", "in", "for", "do", "i", "how"];

    return text.toLowerCase()
        .replace(/[^a-z0-9\s]/g, "")
        .split(/\s+/)
        .filter(word => word && !stopWords.includes(word))
        .map(word => word.length > 3 && word.endsWith("s")
            ? word.slice(0, -1) : word);
}

// Cosine similarity
function cosineSimilarity(text1, text2) {
    const words1 = preprocess(text1);
    const words2 = preprocess(text2);

    const allWords = [...new Set([...words1, ...words2])];

    let dotProduct = 0;
    let magnitude1 = 0;
    let magnitude2 = 0;

    allWords.forEach(word => {
        const count1 = words1.filter(w => w === word).length;
        const count2 = words2.filter(w => w === word).length;

        dotProduct += count1 * count2;
        magnitude1 += count1 * count1;
        magnitude2 += count2 * count2;
    });

    if (magnitude1 === 0 || magnitude2 === 0) {
        return 0;
    }

    return dotProduct /
        (Math.sqrt(magnitude1) * Math.sqrt(magnitude2));
}

// Find the best FAQ answer
function findAnswer(question) {
    let bestAnswer = null;
    let highestScore = 0;

    faqs.forEach(faq => {
        faq.questions.forEach(example => {
            const score = cosineSimilarity(question, example);

            if (score > highestScore) {
                highestScore = score;
                bestAnswer = faq.answer;
            }
        });
    });

    if (highestScore >= 0.30) {
        return bestAnswer;
    }

    return "Sorry, I couldn't understand your question. Please contact your college office.";
}

// Display messages
function addMessage(message, className) {
    const messageDiv = document.createElement("div");
    messageDiv.className = className;
    messageDiv.textContent = message;
    chatBox.appendChild(messageDiv);
    chatBox.scrollTop = chatBox.scrollHeight;
}

// Send question
function sendMessage() {
    const question = userInput.value.trim();

    if (!question) return;

    addMessage(question, "user-message");

    const answer = findAnswer(question);
    addMessage(answer, "bot-message");

    userInput.value = "";
}

sendBtn.addEventListener("click", sendMessage);

userInput.addEventListener("keydown", function(event) {
    if (event.key === "Enter") {
        sendMessage();
    }
});

// Clear conversation
clearBtn.addEventListener("click", function() {
    chatBox.innerHTML = "";

    addMessage(
        "Hello! Welcome to College FAQ Assistant. Ask me anything about college!",
        "bot-message"
    );
});

