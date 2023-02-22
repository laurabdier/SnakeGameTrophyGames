<script>
export default {
  name: "Canvas",
  props: {
    canStartGame: {
      type: Boolean,
      required: true,
    },
    score: {
      type: Number,
      required: true,
    },
    setPlayerScore: {
      type: Function,
      default() {
        return {};
      },
    },
    endGame: {
      type: Function,
      required: true,
    },
    setTimePassed: {
      type: Function,
      required: true,
    },
    stopTimer: {
      type: Function,
      required: true,
    },
  },
  data() {
    return {
      width: 20,
      height: 15,
      cellSize: 20,
      snake: [{ x: 0, y: 0 }],
      nextDirection: null,
      nextFoodPosition: { x: 0, y: 0 },
      directions: [
        {
          // left
          keyCode: [37, 65],
          refKeyCode: 37,
          move: {
            x: -1,
            y: 0,
          },
        },
        {
          // up
          keyCode: [38, 87],
          refKeyCode: 38,
          move: {
            x: 0,
            y: -1,
          },
        },
        {
          // right
          keyCode: [39, 68],
          refKeyCode: 39,
          move: {
            x: 1,
            y: 0,
          },
        },
        {
          // up
          keyCode: [40, 83],
          refKeyCode: 40,
          move: {
            x: 0,
            y: 1,
          },
        },
      ],
    };
  },
  mounted() {
    this.context = this.$refs.canvas.getContext("2d");
    this.reset();
    window.addEventListener("keydown", this.onKeyDown);
    this.interval = setInterval(this.moveNext, 200);
  },
  methods: {
    reset() {
      // create head of the snake only:
      this.nextDirection = null;
      this.snake = [
        {
          x: Math.floor(this.width / 2),
          y: Math.floor(this.height / 2),
        },
      ];

      // compute position of snake's food
      this.computeNewFoodPosition();

      // draw the scene
      this.drawGame();
    },

    drawGame() {
      this.context.clearRect(
        0,
        0,
        this.width * this.cellSize,
        this.height * this.cellSize
      );

      // draw each part of the snake
      this.context.beginPath();
      this.snake.forEach((part) => {
        this.context.rect(
          part.x * this.cellSize,
          part.y * this.cellSize,
          this.cellSize,
          this.cellSize
        );
        this.context.fillStyle = "black";
        this.context.fill();
      });
      this.context.closePath();

      // Draw the food
      this.context.beginPath();
      this.context.rect(
        this.nextFoodPosition.x * this.cellSize,
        this.nextFoodPosition.y * this.cellSize,
        this.cellSize,
        this.cellSize
      );
      this.context.fillStyle = "red";
      this.context.fill();
      this.context.closePath();
    },

    onKeyDown(event) {
      if (!this.canStartGame) {
        return;
      }
      // find the direction
      const direction = this.directions.find((dir) =>
        dir.keyCode.includes(event.keyCode)
      );

      if (!direction) {
        return;
      }

      if (
        this.nextDirection === null ||
        Math.abs(this.nextDirection.refKeyCode - direction.refKeyCode) !== 2
      ) {
        this.nextDirection = direction;

        return;
      }
    },

    moveNext() {
      if (this.nextDirection !== null) {
        // compute the new position
        this.snake.unshift({
          x: this.snake[0].x + this.nextDirection.move.x,
          y: this.snake[0].y + this.nextDirection.move.y,
        });

        // check if the new head == foodPosition
        if (
          this.snake[0].x === this.nextFoodPosition.x &&
          this.snake[0].y === this.nextFoodPosition.y
        ) {
          this.computeNewFoodPosition();
          this.setPlayerScore(this.score + 20);
        } else {
          this.snake.pop();
        }

        // Check if the new head position is not in the snake part or border
        if (
          this.snake[0].x < 0 ||
          this.snake[0].x >= this.width ||
          this.snake[0].y < 0 ||
          this.snake[0].y >= this.height
        ) {
          this.reset();
          const failMessage = "Too bad, you touched the border";
          this.endGame(failMessage);
          return;
        }

        // check part of snake
        for (let cpt = 1; cpt < this.snake.length; cpt++) {
          if (
            this.snake[0].x === this.snake[cpt].x &&
            this.snake[0].y === this.snake[cpt].y
          ) {
            this.reset();
            const failMessage = "Too bad, you touched the snake body";
            this.endGame(failMessage);
            return;
          }
        }

        this.drawGame();
      }

      return;
    },

    computeNewFoodPosition() {
      let positionFound = false;

      while (!positionFound) {
        // Compute random position
        let foodPosition = {
          x: Math.floor(Math.random() * this.width),
          y: Math.floor(Math.random() * this.height),
        };

        // check if this is not a part of the snake
        let snakePart = this.snake.find(
          (snakeP) => snakeP.x === foodPosition.x && snakeP.y === foodPosition.y
        );
        if (!snakePart) {
          this.nextFoodPosition = foodPosition;
          positionFound = true;
        }
      }

      this.drawGame();
    },
  },
};
</script>

<template>
  <div>
    <canvas
      ref="canvas"
      id="”canvas”"
      :width="width * cellSize"
      :height="height * cellSize"
      style="border: 1px solid #bcb1a8"
    ></canvas>
  </div>
</template>
