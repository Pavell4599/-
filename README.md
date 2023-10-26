<!DOCTYPE html>
<html>
<head>
    <title>Чат-бот</title>
    <style>
        .chat-container {
            max-height: 250px;
            overflow-y: scroll;
        }

        .user-message {
            background-color: lightgray;
        }

        .bot-message {
            background-color: lightblue;
        }
    </style>
</head>
<body>
    <div class="chat-container">
        <div id="chat-display"></div>
    </div>
    <div>
        <p>Выберите вопрос:</p>
        <button onclick="selectQuestion('В какие дни и в какое время национальный парк «Куршская коса» открыт для посещения?')">В какие дни и в какое время национальный парк «Куршская коса» открыт для посещения?</button>
        <button onclick="selectQuestion('Как попасть на Куршскую косу? Мы приехали на КПП/в Зеленоградск – куда идти дальше?')">Как попасть на Куршскую косу? Мы приехали на КПП/в Зеленоградск – куда идти дальше?</button>
        <button onclick="selectQuestion('Где на Куршской косе можно разместиться с палаткой?')">Где на Куршской косе можно разместиться с палаткой?</button>
        <button onclick="selectQuestion('Как попасть на экскурсию на Куршскую косу?')">Как попасть на экскурсию на Куршскую косу?        </button>
        <button onclick="selectQuestion('Нужно ли каждый раз покупать входной билет при въезде на территорию национального парка?')">Нужно ли каждый раз покупать входной билет при въезде на территорию национального парка?</button>
        <button onclick="selectQuestion('Если приехать вечером и разместиться на Куршской косе на ночлег, нужно ли на следующий день снова покупать входной билет в парк?')">Если приехать вечером и разместиться на Куршской косе на ночлег, нужно ли на следующий день снова покупать входной билет в парк?</button>
        <button onclick="selectQuestion('Как покупать входные билеты посетителям, въезжающим на рейсовых автобусах, и в каких случаях им можно не покупать билеты?')">Как покупать входные билеты посетителям, въезжающим на рейсовых автобусах, и в каких случаях им можно не покупать билеты?</button>
    </div>
    <div>
        <input type="text" id="user-input" placeholder="Введите ваш вопрос">
        <button onclick="sendUserMessage()">Отправить</button>
    </div>
    <a href="question-counter.html" target="_blank">Посмотреть статистику</a>

    <script>
        var questionCount = 0;
        var questionCounterElement = document.getElementById('question-counter');
        var chatDisplay = document.getElementById('chat-display');

        // Retrieve the stored question count from local storage or initialize it to 0
        questionCount = parseInt(localStorage.getItem('questionCount')) || 0;

        // Display the question count on the page
        questionCounterElement.innerText = 'Всего задано вопросов: ' + questionCount;

        // Check if a new question has been submitted
        if (sessionStorage.getItem('userInput')) {
            // Increment the question count and display it on the page
            questionCount++;
            updateQuestionCounter();

            // Clear the user input from sessionStorage
            sessionStorage.removeItem('userInput');
        }

        function resetQuestionCounter() {
            localStorage.removeItem('questionCount');
            questionCount = 0;
            updateQuestionCounter();
        }

        function selectQuestion(question) {
            updateChatDisplay(question, true);
            if (question === 'В какие дни и в какое время национальный парк «Куршская коса» открыт для посещения?') {
        botResponse('Национальный парк открыт для посещения 24/7 – круглосуточно и круглогодично. Визит-центр, расположенный на 14 км косы, открыт с 9 до 17 часов: с мая по сентябрь ежедневно, с октября по апрель – со среды по воскресенье. Полевой стационар кольцевания птиц открыт для посещения с апреля по октябрь с 10 до 17 часов.');
    } else if (question === 'Как попасть на Куршскую косу? Мы приехали на КПП/в Зеленоградск – куда идти дальше?') {
        botResponse('Для осмотра основных достопримечательностей на Куршской косе требуется транспорт. Можно перемещаться на автомобиле, туристическом или рейсовом автобусе. Есть парковки. Экскурсию в составе туристической группы нужно заказывать заранее в любой из калининградских турфирм, рейсовый автобус № 210 отправляется от вокзала в Зеленоградске, в летний сезон из Калининграда на косу ходит автобус № 593.');
    } else if (question === 'Где на Куршской косе можно разместиться с палаткой?') {
        botResponse('Организовывать туристическую стоянку на Куршской косе запрещено! В дневное время можно ставить палатку или параван на берегу моря для защиты от солнца и ветра. Ставить палатку для ночлега можно только в специально организованных для этого местах: на территории турбаз, туркемпингов и гостевых домов, на пикниковом месте возле озера Чайка (32-й км).');
            } else if (question === 'Как попасть на экскурсию на Куршскую косу?') {
                botResponse('Чтобы посетить национальный парк с экскурсией, нужно заранее записаться в группу в любой из калининградских турфирм или заказать индивидуальную экскурсию на автомобиле/микроавтобусе с выездом из Калининграда или курортных городов области. Экскурсию по залам Визит-центра и тематические занятия для детских групп можно заказать, позвонив в отдел экопросвещения 8 4012 310028.');
            } else if (question === 'Нужно ли каждый раз покупать входной билет при въезде на территорию национального парка?') {
                botResponse('Да, каждый въезд на территорию национального парка необходимо оплачивать и покупать входной билет.');
            } else if (question === 'Если приехать вечером и разместиться на Куршской косе на ночлег, нужно ли на следующий день снова покупать входной билет в парк?') {
                botResponse('Не нужно, билет действует трое суток при условии непрерывного пребывания на территории национального парка. Билет необходимо сохранять до момента выезда с Куршской косы.');
            } else if (question === 'Как покупать входные билеты посетителям, въезжающим на рейсовых автобусах, и в каких случаях им можно не покупать билеты?') {
                botResponse('Пассажиры рейсовых автобусов могут купить входной билет только онлайн – на сайте национального парка для перехода на сайт онлайн-оплаты нужно нажать «оплатить посещение». Для посещения экологических троп, маршрутов и других объектов национального парка это нужно сделать обязательно. Если пассажир едет на Куршскую косу с иными целями, например, в какой-то из поселков, покупать входной билет, в летний сезон из Калининграда на косу ходит автобус № 593.');
            }
        }

        function sendUserMessage() {
            var userInput = document.getElementById('user-input').value.trim();
            if (userInput !== '') {
                updateChatDisplay(userInput, true);

                // Добавим проверку, если это вопрос, на который бот не знает ответа
                if (userInput.includes('?')) {
                    botResponse('Спасибо за ваш вопрос! Вам скоро ответит специалист');
                } else {
                    // Ваш код для обработки сообщения пользователя
                    // ...
                }

                document.getElementById('user-input').value = '';
            }
        }

        function botResponse(message) {
            updateChatDisplay(message, false);
        }

        function updateChatDisplay(message, isUser) {
            var chatMessage = document.createElement('div');

            chatMessage.className = isUser ? 'user-message' : 'bot-message';
            chatMessage.innerText = message;

            chatDisplay.appendChild(chatMessage);
            chatDisplay.scrollTop = chatDisplay.scrollHeight;
        }

        function updateQuestionCounter() {
            questionCounterElement.innerText = 'Всего задано вопросов: ' + questionCount;
            localStorage.setItem('questionCount', questionCount.toString());
        }
    </script>
    

</body>
</html>
/* styles.css */
.chat-container {
    position: fixed;
    bottom: 120px;
    left: 0;
    right: 0;
    top: 0;
    padding: 10px;
    overflow-y: scroll;
}

.chat-messages {
    margin-bottom: 10px;
}

.user-input-container {
    position: fixed;
    bottom: 0;
    left: 0;
    right: 0;
    height: 100px;
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.user-message {
    background-color: red;
    color: white;
    padding: 5px;
    margin-bottom: 10px;
    border-radius: 5px;
    max-width: 70%;
    align-self: flex-start; /* изменение выравнивания для текста вопроса */
}

.bot-message {
    background-color: blue;
    color: white;
    padding: 5px;
    margin-bottom: 10px;
    border-radius: 5px;
    max-width: 70%;
    align-self: flex-end; /* изменение выравнивания для текста ответа */
}
<!DOCTYPE html>
<html>
<head>
  <title>Страница счетчика вопросов</title>
</head>
<body>
  <h1>Страница счетчика вопросов</h1>
  
  <div id="questionCounter"></div>
  
  <script>
    let userQuestionCount = 0;

    function selectQuestion(question) {
      userQuestionCount += 1;
      updateQuestionCounter();

      const answerInput = document.createElement('input');
      answerInput.setAttribute('type', 'text');
      answerInput.setAttribute('placeholder', 'Введите ответ...');
      answerInput.setAttribute('id', `answerInput${userQuestionCount}`);
      answerInput.setAttribute('class', 'answer-input');

      const answerButton = document.createElement('button');
      answerButton.innerText = 'Ответить';
      answerButton.onclick = function() {
        getAnswer(userQuestionCount);
      };

      const questionElement = document.createElement('p');
      questionElement.innerText = question;

      const container = document.createElement('div');
      container.setAttribute('class', 'question-container');
      container.appendChild(questionElement);
      container.appendChild(answerInput);
      container.appendChild(answerButton);

      document.getElementById('questionCounter').appendChild(container);
    }

    function getAnswer(questionNumber) {
      const userAnswer = document.getElementById(`answerInput${questionNumber}`).value;
      const userQuestion = document.getElementById(`question${questionNumber}`).innerText;

      const answerButton = document.createElement('button');
      answerButton.innerText = userAnswer;
      answerButton.onclick = function() {
        sendUserMessage(userQuestion);
      };

      const questionElement = document.createElement('p');
      questionElement.setAttribute('id', `question${questionNumber}`);
      questionElement.innerText = userQuestion;

      document.getElementById('chat').appendChild(answerButton);
    }

    function sendUserMessage(question) {
      const questionNumber = userQuestionCount + 1;
      const questionElement = document.createElement('p');
      questionElement.setAttribute('id', `question${questionNumber}`);
      questionElement.innerText = question;

      document.getElementById('chat').appendChild(questionElement);
    }

    function updateQuestionCounter() {
      document.getElementById('questionCounter').innerText = `Вопросов: ${userQuestionCount}`;
    }
  </script>
</body>
</html>
var questionCount = 0;
var questionCounterElement = document.getElementById('question-counter');

// Retrieve the question count from local storage or initialize it to 0
var questionCount = parseInt(localStorage.getItem('questionCount')) || 0;

// Display the question count on the page
questionCounterElement.innerText = 'Всего задано вопросов: ' + questionCount;

function resetQuestionCounter() {
  localStorage.removeItem('questionCount');
  questionCount = 0;
  questionCounterElement.innerText = 'Всего задано вопросов: ' + questionCount;
}
# -
