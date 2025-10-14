// ==================== SCREEN MANAGEMENT ====================
let currentScreen = "menu"
let currentGame = null

function showScreen(screenName) {
  // Stop current game if any
  if (currentGame && currentGame.stop) {
    currentGame.stop()
  }

  // Hide all screens
  document.querySelectorAll(".screen").forEach((screen) => {
    screen.classList.remove("active")
  })

  // Show requested screen
  const screen = document.getElementById(screenName + "Screen")
  if (screen) {
    screen.classList.add("active")
    currentScreen = screenName

    // Initialize game if needed
    if (screenName === "snake") {
      initSnakeGame()
    } else if (screenName === "tetris") {
      console.error("initTetrisGame is not defined")
    } else if (screenName === "pong") {
      console.error("initPongGame is not defined")
    } else if (screenName === "memory") {
      console.error("initMemoryGame is not defined")
    }
  }
}

// ==================== MENU SCREEN ====================
document.addEventListener("DOMContentLoaded", () => {
  const gameCards = document.querySelectorAll(".game-card")

  gameCards.forEach((card) => {
    const playButton = card.querySelector(".play-button")
    const gameName = card.dataset.game

    playButton.addEventListener("click", (e) => {
      e.stopPropagation()
      showScreen(gameName)
    })

    card.addEventListener("click", () => {
      showScreen(gameName)
    })
  })

  animateCards()
})

function animateCards() {
  const cards = document.querySelectorAll(".game-card")

  cards.forEach((card, index) => {
    card.style.opacity = "0"
    card.style.transform = "translateY(30px)"

    setTimeout(() => {
      card.style.transition = "all 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275)"
      card.style.opacity = "1"
      card.style.transform = "translateY(0)"
    }, 100 * index)
  })
}

// ==================== SNAKE GAME ====================
let snakeGame = null

function initSnakeGame() {
  const canvas = document.getElementById("snakeCanvas")
  const ctx = canvas.getContext("2d")
  const scoreElement = document.getElementById("snakeScore")
  const highScoreElement = document.getElementById("snakeHighScore")
  const overlay = document.getElementById("snakeOverlay")
  const overlayTitle = document.getElementById("snakeOverlayTitle")
  const overlayMessage = document.getElementById("snakeOverlayMessage")
  const startButton = document.getElementById("snakeStartButton")

  const GRID_SIZE = 20
  const CANVAS_SIZE = 600
  canvas.width = CANVAS_SIZE
  canvas.height = CANVAS_SIZE

  let snake = []
  let direction = { x: 1, y: 0 }
  let nextDirection = { x: 1, y: 0 }
  let food = { x: 0, y: 0 }
  let score = 0
  let highScore = localStorage.getItem("snakeHighScore") || 0
  let gameRunning = false
  let gameLoop = null
  let touchStartX = 0
  let touchStartY = 0

  highScoreElement.textContent = highScore

  function init() {
    snake = [
      { x: 10, y: 10 },
      { x: 9, y: 10 },
      { x: 8, y: 10 },
    ]
    direction = { x: 1, y: 0 }
    nextDirection = { x: 1, y: 0 }
    score = 0
    scoreElement.textContent = score
    spawnFood()
  }

  function spawnFood() {
    food = {
      x: Math.floor(Math.random() * (CANVAS_SIZE / GRID_SIZE)),
      y: Math.floor(Math.random() * (CANVAS_SIZE / GRID_SIZE)),
    }

    for (const segment of snake) {
      if (segment.x === food.x && segment.y === food.y) {
        spawnFood()
        return
      }
    }
  }

  function start() {
    if (gameRunning) return

    init()
    gameRunning = true
    overlay.classList.add("hidden")

    if (gameLoop) clearInterval(gameLoop)
    gameLoop = setInterval(update, 100)
  }

  function update() {
    direction = { ...nextDirection }

    const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y }

    if (head.x < 0 || head.x >= CANVAS_SIZE / GRID_SIZE || head.y < 0 || head.y >= CANVAS_SIZE / GRID_SIZE) {
      gameOver()
      return
    }

    for (const segment of snake) {
      if (segment.x === head.x && segment.y === head.y) {
        gameOver()
        return
      }
    }

    snake.unshift(head)

    if (head.x === food.x && head.y === food.y) {
      score += 10
      scoreElement.textContent = score
      spawnFood()

      if (score > highScore) {
        highScore = score
        highScoreElement.textContent = highScore
        localStorage.setItem("snakeHighScore", highScore)
      }
    } else {
      snake.pop()
    }

    draw()
  }

  function draw() {
    ctx.fillStyle = "#0f0f1e"
    ctx.fillRect(0, 0, CANVAS_SIZE, CANVAS_SIZE)

    ctx.strokeStyle = "rgba(255, 255, 255, 0.05)"
    ctx.lineWidth = 1
    for (let i = 0; i <= CANVAS_SIZE / GRID_SIZE; i++) {
      ctx.beginPath()
      ctx.moveTo(i * GRID_SIZE, 0)
      ctx.lineTo(i * GRID_SIZE, CANVAS_SIZE)
      ctx.stroke()

      ctx.beginPath()
      ctx.moveTo(0, i * GRID_SIZE)
      ctx.lineTo(CANVAS_SIZE, i * GRID_SIZE)
      ctx.stroke()
    }

    snake.forEach((segment, index) => {
      const gradient = ctx.createLinearGradient(
        segment.x * GRID_SIZE,
        segment.y * GRID_SIZE,
        (segment.x + 1) * GRID_SIZE,
        (segment.y + 1) * GRID_SIZE,
      )

      if (index === 0) {
        gradient.addColorStop(0, "#ff6b35")
        gradient.addColorStop(1, "#ff8c5a")
      } else {
        const alpha = 1 - index / snake.length
        gradient.addColorStop(0, `rgba(255, 107, 53, ${alpha})`)
        gradient.addColorStop(1, `rgba(255, 140, 90, ${alpha})`)
      }

      ctx.fillStyle = gradient
      ctx.fillRect(segment.x * GRID_SIZE + 2, segment.y * GRID_SIZE + 2, GRID_SIZE - 4, GRID_SIZE - 4)

      if (index === 0) {
        ctx.fillStyle = "#ffffff"
        const eyeSize = 3
        const eyeOffset = 6

        if (direction.x === 1) {
          ctx.fillRect(segment.x * GRID_SIZE + GRID_SIZE - eyeOffset, segment.y * GRID_SIZE + 5, eyeSize, eyeSize)
          ctx.fillRect(
            segment.x * GRID_SIZE + GRID_SIZE - eyeOffset,
            segment.y * GRID_SIZE + GRID_SIZE - 8,
            eyeSize,
            eyeSize,
          )
        } else if (direction.x === -1) {
          ctx.fillRect(segment.x * GRID_SIZE + eyeOffset - eyeSize, segment.y * GRID_SIZE + 5, eyeSize, eyeSize)
          ctx.fillRect(
            segment.x * GRID_SIZE + eyeOffset - eyeSize,
            segment.y * GRID_SIZE + GRID_SIZE - 8,
            eyeSize,
            eyeSize,
          )
        } else if (direction.y === -1) {
          ctx.fillRect(segment.x * GRID_SIZE + 5, segment.y * GRID_SIZE + eyeOffset - eyeSize, eyeSize, eyeSize)
          ctx.fillRect(
            segment.x * GRID_SIZE + GRID_SIZE - 8,
            segment.y * GRID_SIZE + eyeOffset - eyeSize,
            eyeSize,
            eyeSize,
          )
        } else {
          ctx.fillRect(segment.x * GRID_SIZE + 5, segment.y * GRID_SIZE + GRID_SIZE - eyeOffset, eyeSize, eyeSize)
          ctx.fillRect(
            segment.x * GRID_SIZE + GRID_SIZE - 8,
            segment.y * GRID_SIZE + GRID_SIZE - eyeOffset,
            eyeSize,
            eyeSize,
          )
        }
      }
    })

    const foodGradient = ctx.createRadialGradient(
      food.x * GRID_SIZE + GRID_SIZE / 2,
      food.y * GRID_SIZE + GRID_SIZE / 2,
      0,
      food.x * GRID_SIZE + GRID_SIZE / 2,
      food.y * GRID_SIZE + GRID_SIZE / 2,
      GRID_SIZE,
    )
    foodGradient.addColorStop(0, "#ffd23f")
    foodGradient.addColorStop(1, "rgba(255, 210, 63, 0)")

    ctx.fillStyle = foodGradient
    ctx.fillRect(food.x * GRID_SIZE - GRID_SIZE / 2, food.y * GRID_SIZE - GRID_SIZE / 2, GRID_SIZE * 2, GRID_SIZE * 2)

    ctx.fillStyle = "#ffd23f"
    ctx.beginPath()
    ctx.arc(food.x * GRID_SIZE + GRID_SIZE / 2, food.y * GRID_SIZE + GRID_SIZE / 2, GRID_SIZE / 2 - 2, 0, Math.PI * 2)
    ctx.fill()
  }

  function gameOver() {
    gameRunning = false
    clearInterval(gameLoop)

    overlayTitle.textContent = "Game Over!"
    overlayMessage.textContent = `Pontuação: ${score}`
    overlay.classList.remove("hidden")
  }

  function handleKeyDown(e) {
    if (currentScreen !== "snake") return

    if (e.code === "Space" && !gameRunning) {
      start()
      return
    }

    if (!gameRunning) return

    switch (e.key) {
      case "ArrowUp":
      case "w":
      case "W":
        if (direction.y === 0) nextDirection = { x: 0, y: -1 }
        break
      case "ArrowDown":
      case "s":
      case "S":
        if (direction.y === 0) nextDirection = { x: 0, y: 1 }
        break
      case "ArrowLeft":
      case "a":
      case "A":
        if (direction.x === 0) nextDirection = { x: -1, y: 0 }
        break
      case "ArrowRight":
      case "d":
      case "D":
        if (direction.x === 0) nextDirection = { x: 1, y: 0 }
        break
    }
  }

  function handleTouchStart(e) {
    e.preventDefault()
    touchStartX = e.touches[0].clientX
    touchStartY = e.touches[0].clientY
  }

  function handleTouchEnd(e) {
    e.preventDefault()
    if (!gameRunning) return

    const touchEndX = e.changedTouches[0].clientX
    const touchEndY = e.changedTouches[0].clientY

    const deltaX = touchEndX - touchStartX
    const deltaY = touchEndY - touchStartY

    if (Math.abs(deltaX) > Math.abs(deltaY)) {
      if (deltaX > 0 && direction.x === 0) {
        nextDirection = { x: 1, y: 0 }
      } else if (deltaX < 0 && direction.x === 0) {
        nextDirection = { x: -1, y: 0 }
      }
    } else {
      if (deltaY > 0 && direction.y === 0) {
        nextDirection = { x: 0, y: 1 }
      } else if (deltaY < 0 && direction.y === 0) {
        nextDirection = { x: 0, y: -1 }
      }
    }
  }

  document.addEventListener("keydown", handleKeyDown)
  canvas.addEventListener("touchstart", handleTouchStart)
  canvas.addEventListener("touchend", handleTouchEnd)
  startButton.addEventListener("click", start)

  init()
  draw()

  snakeGame = {
    stop: () => {
      gameRunning = false
      if (gameLoop) clearInterval(gameLoop)
    },
  }

  currentGame = snakeGame
}

// Placeholder for other game initializations
function initTetrisGame() {
  console.error("Tetris game initialization not implemented")
}

function initPongGame() {
  console.error("Pong game initialization not implemented")
}

function initMemoryGame() {
  console.error("Memory game initialization not implemented")
}
