<script setup>
import { ref } from "vue";

const board = ref(Array(9).fill(null));
const currentPlayer = ref("X");
const statusText = ref("Current Turn: X");
const buttons = [0, 1, 2, 3, 4, 5, 6, 7, 8];

const winPatterns = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
];

const checkWinner = () => {
    for (let i = 0; i < winPatterns.length; i++) {
        const [a, b, c] = winPatterns[i];
        if (
            board.value[a] &&
            board.value[a] === board.value[b] &&
            board.value[a] === board.value[c]
        ) {
            return board.value[a];
        }
    }
    return null;
};

const handleClick = (index) => {
    if (board.value[index] || checkWinner()) {
        return;
    }

    board.value[index] = currentPlayer.value;
    updateStatus(checkWinner());
};

const updateStatus = (winner) => {
    if (winner) {
        statusText.value = `Winner: ${winner}`;
    } else if (board.value.every((cell) => cell)) {
        statusText.value = "Draw";
    } else {
        currentPlayer.value = currentPlayer.value === "X" ? "O" : "X";
        statusText.value = `Current Turn: ${currentPlayer.value}`;
    }
};

const restart = () => {
    board.value = Array(9).fill(null);
    currentPlayer.value = "X";
    statusText.value = "Current Turn: X";
};
</script>

<template>
    <div class="container">
        <div class="turnIndicator">
            <template v-if="statusText.startsWith('Winner')">
                {{ statusText }}
            </template>
            <template v-else-if="statusText === 'Draw'">
                {{ statusText }}
            </template>
            <template v-else>
                {{ statusText }}
            </template>
        </div>
        <div class="btn-display">
            <section class="btn-holder">
                <template v-for="button in buttons">
                    <div class="btn" id="button" @click="handleClick(button)">
                        {{ board[button] }}
                    </div>
                </template>
            </section>
        </div>
        <div class="performance">
            <button class="reset-btn" @click="restart">Reset</button>
        </div>
        <div class="footer">Developed By Alireza Gosili - 2024</div>
    </div>
</template>

<style>
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@400;600;700&display=swap");

:root {
    --primary-color: #6200ea;
    --secondary-color: #3700b3;
    --accent-color: #03dac6;
    --background-color: #121212;
    --surface-color: #1e1e1e;
    --text-color: #ffffff;
    --btn-text-color: #ffffff;
}

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    background-color: var(--background-color);
    color: var(--text-color);
    font-family: "Montserrat", sans-serif;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 100vh;
}

.container {
    width: 100%;
    max-width: 800px;
    padding: 30px;
    background-color: var(--surface-color);
    border-radius: 30px;
    box-shadow: 0 20px 40px rgba(0, 0, 0, 0.3);
}

.turnIndicator {
    text-align: center;
    margin-bottom: 30px;
    font-size: 36px;
    font-weight: 700;
    color: var(--accent-color);
    text-transform: uppercase;
    letter-spacing: 2px;
    height: 44px;
    line-height: 44px;
}

.btn-display {
    background-color: var(--background-color);
    padding: 30px;
    border-radius: 20px;
    margin-bottom: 30px;
}

.btn-holder {
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    gap: 20px;
    aspect-ratio: 1 / 1;
}

.btn {
    aspect-ratio: 1 / 1;
    border-radius: 15px;
    cursor: pointer;
    font-size: 72px;
    font-weight: 600;
    color: var(--btn-text-color);
    background-color: var(--primary-color);
    box-shadow: 0 8px 16px rgba(0, 0, 0, 0.2);
    transition: all 0.3s ease;
    display: flex;
    justify-content: center;
    align-items: center;
    min-height: 120px;
    position: relative;
}

.btn::before {
    content: "";
    display: block;
    padding-top: 100%;
}

.btn span {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

.btn:hover {
    transform: translateY(-5px);
    box-shadow: 0 12px 20px rgba(0, 0, 0, 0.3);
    background-color: var(--secondary-color);
}

.performance {
    display: flex;
    justify-content: center;
}

.reset-btn {
    width: 50%;
    padding: 20px;
    border: none;
    border-radius: 50px;
    cursor: pointer;
    background-color: var(--accent-color);
    color: var(--background-color);
    font-size: 24px;
    font-weight: 600;
    text-transform: uppercase;
    letter-spacing: 1px;
    transition: all 0.3s ease;
}

.reset-btn:hover {
    background-color: #04b8a5;
    transform: translateY(-3px);
    box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
}
.footer {
    text-align: center;
    margin-top: 30px;
    padding: 20px;
    font-size: 16px;
    color: var(--text-color);
    opacity: 0.7;
    border-top: 1px solid rgba(255, 255, 255, 0.1);
}

@media (max-width: 768px) {
    .container {
        max-width: 90%;
        padding: 20px;
    }

    .turnIndicator {
        font-size: 28px;
        height: 34px;
        line-height: 34px;
    }

    .btn {
        font-size: 48px;
        min-height: 80px;
    }

    .reset-btn {
        width: 70%;
        font-size: 20px;
    }
}

@media (max-width: 480px) {
    .container {
        padding: 15px;
    }

    .turnIndicator {
        font-size: 24px;
        height: 30px;
        line-height: 30px;
    }

    .btn {
        font-size: 36px;
        min-height: 60px;
    }
    .btn-display{
        padding: 15px;
    }
    .btn-holder{
        gap: 10px;
    }
    .reset-btn {
        width: 100%;
        font-size: 18px;
    }
    .footer {
        font-size: 14px;
        margin-top: 20px;
        padding: 15px;
    }
}
</style>
