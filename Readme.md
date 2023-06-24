# Asteroid Invader

This is a simple 2D space shooter game implemented using JavaScript and HTML5 canvas. The game features a player-controlled spaceship that fires projectiles to destroy incoming asteroids.

## General Info
In this game, the player controls a spaceship that can rotate and shoot projectiles. The spaceship is placed at the center of the screen and can rotate 360 degrees. The main objective of the game is to shoot and destroy the incoming asteroids. These asteroids come from random directions and at random intervals.

## Technologies
- JavaScript ES6: The entire game logic is built using ES6.
- HTML5: The game is rendered on the web using HTML5.
- HTML5 Canvas: Used for 2D rendering.
- CSS: Used for basic styling of the game container.

## Features
- Player-controlled Spaceship: Use 'W' to accelerate, 'A' to rotate left, 'D' to rotate right, and 'Space' to shoot projectiles.
- Asteroids: Asteroids spawn at random intervals from the edge of the canvas and move in a straight line towards the opposite edge.
- Collisions: The game handles collisions between asteroids and projectiles, as well as collisions between asteroids and the player. A collision between a projectile and an asteroid destroys both, whereas a collision between the player and an asteroid ends the game.

## Setup
Clone this repository.

Open index.html in your web browser.

Start playing the game!

## How the Game Works
The game is essentially built on three main classes: Player, Projectile, and Asteroid.

The Player class is responsible for creating and managing the player's spaceship. This includes its position, velocity, and rotation, as well as drawing the spaceship on the canvas and updating its position based on the velocity and user input.

The Projectile class handles the projectiles shot by the player's spaceship. Each projectile is an instance of this class and contains information about its position and velocity. It also includes methods to draw itself on the canvas and update its position based on its velocity.

The Asteroid class manages the asteroids in the game. Similar to the Projectile class, each asteroid contains information about its position, velocity, and radius, and has methods to draw itself on the canvas and update its position.

One of the main aspects of the game is collision detection. The game checks for collisions between projectiles and asteroids, and between the player's spaceship and asteroids. When a collision between a projectile and an asteroid is detected, both are removed from the game. When a collision between the player's spaceship and an asteroid is detected, the game ends.

A crucial function animate serves as the game loop. In each iteration of the loop, it updates the state of all game objects (player, projectiles, and asteroids), handles collisions, and re-renders all game objects.

User input is handled by listening for keydown and keyup events. When the user presses a key, the corresponding state in the keys object is updated, which affects the spaceship's motion and ability to shoot projectiles.

The game also features randomly spawning asteroids that move towards the player from the edges of the screen. This is achieved through a setInterval function that creates new Asteroid objects at random intervals.

## Code Examples

Let's dive into some key aspects of the codebase:

Player Class: Represents the spaceship controlled by the player. It is initialized with position and velocity vectors, and it has a rotation angle. The Player class has methods to draw the spaceship on the canvas, update its state, and calculate its vertices for collision detection.

Projectile Class: Represents the projectiles shot by the spaceship. It is initialized with position and velocity vectors, and it has a method to draw the projectile on the canvas and update its state.

Asteroid Class: Represents the asteroids that the spaceship must destroy. It is initialized with position and velocity vectors and a radius. The Asteroid class has methods to draw the asteroid on the canvas and update its state.

Collision Detection: The game uses circle-circle collision detection for collisions between asteroids and projectiles, and circle-polygon collision detection for collisions between asteroids and the spaceship.

Game Loop: The game loop is implemented using requestAnimationFrame. During each iteration of the game loop, the game updates the state of all game objects, handles collisions, and re-renders all game objects.

Garbage Collection for Projectiles and Asteroids: Garbage collection in the context of this game involves removing projectiles and asteroids that are no longer needed. This is crucial for maintaining optimal performance as it helps free up memory that the game doesn't need to use anymore.

### Geometry and Trigonometry in Asteroid Invader
Geometry and Trigonometry are both critical to the functioning of our Space Shooter Game. They are used to calculate distances, angles, and positions for various elements like the player's spaceship, projectiles, and asteroids.

### Player's Spaceship Rotation and Movement
In our game, the player's spaceship rotates and moves based on the player's input. The rotation and movement of the spaceship are handled using Trigonometry.

When the player presses 'W' to accelerate, we need to figure out how far the spaceship moves in the x and y directions based on its current rotation. For this, we use the Math.cos() and Math.sin() functions, which respectively return the cosine and sine of an angle.

```
if (keys.w.pressed) {
  player.velocity.x = Math.cos(player.rotation) * SPEED
  player.velocity.y = Math.sin(player.rotation) * SPEED
}
```
Cosine gives us the x-component (horizontal direction), and sine gives us the y-component (vertical direction) of the velocity. Multiplying these by the speed gives us the velocity in the x and y directions.

### Projectile's Trajectory
Similar to the player's spaceship, the trajectory of the projectiles is also calculated using Trigonometry.

When a projectile is fired, it travels in the direction that the spaceship is currently facing. We use the Math.cos() and Math.sin() functions again to get the x and y velocities of the projectile. This ensures that the projectile moves along the line of sight of the spaceship.
```
new Projectile({
  position: {
    x: player.position.x + Math.cos(player.rotation) * 30,
    y: player.position.y + Math.sin(player.rotation) * 30,
  },
  velocity: {
    x: Math.cos(player.rotation) * PROJECTILE_SPEED,
    y: Math.sin(player.rotation) * PROJECTILE_SPEED,
  },
})
```
### Collision Detection
In this game, we have two types of collision detection: circle-circle and circle-triangle. These checks ensure that our game responds accurately when the player's spaceship or a projectile comes into contact with an asteroid.

#### Circle-Circle Collision Detection
The game checks for collisions between the player's projectiles and the asteroids. Both of these objects are represented as circles. The following function checks if the distance between the centers of two circles is less than or equal to the sum of their radii, indicating a collision:

```
function circleCollision(circle1, circle2) {
  const xDifference = circle2.position.x - circle1.position.x
  const yDifference = circle2.position.y - circle1.position.y

  const distance = Math.sqrt(
    xDifference * xDifference + yDifference * yDifference
  )

  if (distance <= circle1.radius + circle2.radius) {
    return true
  }

  return false
}
```

This function is used within the asteroid management loop to check for collisions between each asteroid and each projectile:

```
for (let j = projectiles.length - 1; j >= 0; j--) {
  const projectile = projectiles[j]

  if (circleCollision(asteroid, projectile)) {
    asteroids.splice(i, 1)
    projectiles.splice(j, 1)
  }
}
```
#### Circle-Triangle Collision Detection
The game also checks for collisions between the player's spaceship (a triangle) and the asteroids (circles). Here's the function that performs this check:

```
function circleTriangleCollision(circle, triangle) {
  for (let i = 0; i < 3; i++) {
    let start = triangle[i]
    let end = triangle[(i + 1) % 3]

    let dx = end.x - start.x
    let dy = end.y - start.y
    let length = Math.sqrt(dx * dx + dy * dy)

    let dot =
      ((circle.position.x - start.x) * dx +
        (circle.position.y - start.y) * dy) /
      Math.pow(length, 2)

    let closestX = start.x + dot * dx
    let closestY = start.y + dot * dy

    if (!isPointOnLineSegment(closestX, closestY, start, end)) {
      closestX = closestX < start.x ? start.x : end.x
      closestY = closestY < start.y ? start.y : end.y
    }

    dx = closestX - circle.position.x
    dy = closestY - circle.position.y

    let distance = Math.sqrt(dx * dx + dy * dy)

    if (distance <= circle.radius) {
      return true
    }
  }

  // No collision
  return false
}
```

This function is also used within the asteroid management loop, this time to check for collisions between each asteroid and the player's spaceship:

```
if (circleTriangleCollision(asteroid, player.getVertices())) {
  console.log('GAME OVER')
  window.cancelAnimationFrame(animationId)
  clearInterval(intervalId)
}
```
Through these collision detection functions, we are able to have responsive and interactive game mechanics.

### Drawing the Player's Spaceship and Asteroids

Both Geometry and Trigonometry are also used to draw the player's spaceship and asteroids on the canvas. The spaceship is a triangle that rotates based on the player's input, and the asteroids are circles with random radii.

In the case of the spaceship, the vertices of the triangle need to rotate around the center of the spaceship. This is achieved using rotation matrix transformations, which involve Trigonometry.

In the case of the asteroids, each one is drawn as a circle using the arc() function, which involves Geometry. The position and radius of each asteroid are randomly generated.

In summary, Geometry and Trigonometry are fundamental to the creation and functioning of the game, enabling the dynamic and interactive features that make the game fun and engaging.

### Garbage Collection for Projectiles and Asteroids
In the game implementation, an essential aspect of memory management involves garbage collection for both projectiles and asteroids. We need to remove objects that are no longer in play to free up memory, ensuring that our game performs efficiently.

Below are the specific code examples related to this feature.

#### Garbage Collection for Projectiles
Here, we're looping through each projectile and checking if it's still within the visible canvas area. If it isn't, we remove it from the projectiles array
```
for (let i = projectiles.length - 1; i >= 0; i--) {
  const projectile = projectiles[i]
  projectile.update()

  // Check if projectile is off screen
  if (
    projectile.position.x + projectile.radius < 0 ||
    projectile.position.x - projectile.radius > canvas.width ||
    projectile.position.y - projectile.radius > canvas.height ||
    projectile.position.y + projectile.radius < 0
  ) {
    // If so, remove it from the array
    projectiles.splice(i, 1)
  }
}
```
By removing the reference to the projectile from our array, we mark it for garbage collection. This mechanism ensures that the memory used by the projectile can be reused.

#### Garbage Collection for Asteroids
Garbage collection for asteroids follows a similar approach. Here, we also remove asteroids that are off the screen. Additionally, we remove both the asteroid and the projectile from their respective arrays when a collision occurs:
```
for (let i = asteroids.length - 1; i >= 0; i--) {
  const asteroid = asteroids[i]
  asteroid.update()

  // Check if asteroid is off screen
  if (
    asteroid.position.x + asteroid.radius < 0 ||
    asteroid.position.x - asteroid.radius > canvas.width ||
    asteroid.position.y - asteroid.radius > canvas.height ||
    asteroid.position.y + asteroid.radius < 0
  ) {
    // If so, remove it from the array
    asteroids.splice(i, 1)
  }

  // Check for collisions with projectiles
  for (let j = projectiles.length - 1; j >= 0; j--) {
    const projectile = projectiles[j]

    if (circleCollision(asteroid, projectile)) {
      // If collision, remove both asteroid and projectile from arrays
      asteroids.splice(i, 1)
      projectiles.splice(j, 1)
    }
  }
}
```
This code helps us maintain the efficient performance of our game by only keeping relevant objects in memory and marking the rest for garbage collection.
