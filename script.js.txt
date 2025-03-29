let players = [];
let currentPlayer = 0;
let marvelCharacters = [];
let timer;
let timeLeft = 20;

const characterInput = document.getElementById('character-input');
const submitButton = document.getElementById('submit-button');
const currentTurn = document.getElementById('current-turn');
const timeDisplay = document.getElementById('time');
const gameResult = document.getElementById('game-result');
const namedCharactersList = document.getElementById('named-characters');
const startGameButton = document.getElementById('start-game');
const playerSetup = document.getElementById('player-setup');
const gameContainer = document.getElementById('game-container');
const playerNamesDiv = document.getElementById('player-names');
const numPlayersSelect = document.getElementById('num-players');

// Generate player input fields dynamically
numPlayersSelect.addEventListener('change', () => {
    const numPlayers = parseInt(numPlayersSelect.value);
    playerNamesDiv.innerHTML = ''; // Clear existing inputs
    for (let i = 1; i <= numPlayers; i++) {
        const input = document.createElement('input');
        input.type = 'text';
        input.id = `player${i}-name`;
        input.placeholder = `Player ${i} Name`;
        playerNamesDiv.appendChild(input);
    }
});

// Start the game when player names are submitted
startGameButton.addEventListener('click', () => {
    const numPlayers = parseInt(numPlayersSelect.value);
    players = [];
    for (let i = 1; i <= numPlayers; i++) {
        const playerName = document.getElementById(`player${i}-name`).value || `Player ${i}`;
        players.push(playerName);
    }

    if (players.length >= 2) {
        currentPlayer = 0;
        currentTurn.textContent = `${players[currentPlayer]}'s turn to name a character`;
        playerSetup.style.display = 'none';
        gameContainer.style.display = 'block';
        startTimer();
    }
});

function startTimer() {
    clearInterval(timer);
    timeLeft = 20;
    timeDisplay.textContent = timeLeft;
    timer = setInterval(() => {
        timeLeft--;
        timeDisplay.textContent = timeLeft;
        if (timeLeft <= 0) {
            clearInterval(timer);
            gameResult.textContent = `${players[currentPlayer]} loses! Time ran out!`;
            submitButton.disabled = true;
        }
    }, 1000);
}

// Handle character submission
submitButton.addEventListener('click', () => {
    const character = characterInput.value.trim().toLowerCase();
    if (character && !marvelCharacters.includes(character)) {
        marvelCharacters.push(character);
        addCharacterToList(character); // Update list
        characterInput.value = '';
        currentPlayer = (currentPlayer + 1) % players.length; // Switch to the next player
        currentTurn.textContent = `${players[currentPlayer]}'s turn to name a character`;
        startTimer();
    } else if (marvelCharacters.includes(character)) {
        gameResult.textContent = "That character has already been named!";
    }
});

// Update the named characters list
function addCharacterToList(character) {
    const listItem = document.createElement('li');
    listItem.textContent = character;
    namedCharactersList.appendChild(listItem);
}

startTimer();
