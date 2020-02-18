# Project-1: Space Invaders

### ![](https://cloud.githubusercontent.com/assets/40461/8183776/469f976e-1432-11e5-8199-6ac91363302b.png) General Assembly, Software Engineering Immersive

## Overview
Link to the game on [GitHub Pages](https://charlotte-thomas.github.io/project-1/)

The first project tasked to us by General Assembly was to render a grid-based retro arcade game in the browser using JavaScript for DOM manipulation. I chose to build the game ‘Space Invaders’ as it was the game I was most familiar with from the list.

Space Invaders is a classic arcade game from the 80s. The player aims to shoot the invading wave of aliens, before it reaches the planet's surface.

This was a **week-long solo project**. 


<p align="center">
  <img width="600" height="600" src="https://media.giphy.com/media/fuQ7CXvE4cloi8AooU/giphy.gif">
</p>

## Brief
For this particular game, there were two mandatory requirements which needed to be met in order to create the MVP. 

These were:

1. The player should be able to clear at least one wave of aliens.
2. The player's score should be displayed at the end of the game.

Other features of the game include:
Being able to move the player ship left and right 
ability to shoot and destroy aliens with bullets 
for aliens to drop bombs which can hit the player and remove lives 
for aliens to move in the classic style, whereby, they all move in synchonocity until they reach the edge of the grid and then they move down one grid-space before moving the opposite way across the gid.
lives to be lost when aliens reach the bottom level of the grid.
For the game a game over feature to appear if all lives are lost and a winning message to appear if the wave of aliens is defeated.



## Technologies Used

> Vanilla JavaScript (ES6)  
> HTML5   
> CSS3   
> Git  
> GitHub

## Approach Taken

### Initial-Steps
Prior to my project idea being signed-off by a General Assembly Instructor, I wrote pseudocode for each of the main elements of the game. This was done in order to break-down the logic necessary for each one.

These elements included:  
- Player / spaceship movement   
- Alien movement   
- Bullet logic & movement   
- Bomb logic & movement   
- Collision logic

### Grid Development 
I decided to make a reasonably large grid as I suspected that animations would look smoother if there were more grid cells. 

I therefore made a grid that was 25 x 25 (625 cells).

### Spaceship

I placed the spaceship at the bottom of the grid and in the center.
The spaceship (controlled by the player) can either move left or right. I decided to use the keys A (left) and D (right) for movement.
The logic for spaceship movement involved the use of a key-up event listener and switch statement.

For movement of all images in the game, classes where added and removed from the cell divs.

This was also included for bullet logic, where the spacebar initiated the firing of a bullet from the spaceships current position.


	document.addEventListener('keyup', (e) => {          
	    switch (e.keyCode) {
	      case 65: {
	        if (ship === 600) {
	          return
	        }
	        cells[ship].classList.remove('ship')
	        ship = ship - 1
	        cells[ship].classList.add('ship')
	        break
	      }
	      case 68: {
	        if (ship === 624) {
	          return
	        }
	        cells[ship].classList.remove('ship')
	        ship = ship + 1
	        cells[ship].classList.add('ship')
	        break
	      }
	      case 32: {
	        bullet = ship - width
	        cells[bullet].classList.add('bullet')
	        bulletArrayOfArrays.push([bullet])
	        bulletShot()
	        bulletsShot += 1
	        break
	      }
	    }
      })

### Aliens
Alien movement was one of the more challenging features of the game.
The main requirement needed for this movement to be continuous was a set interval function.
For every single cell movement, a number of if statments are run in order to determine the direction of that movement.
	
	function calculateDirection() {
	    if (rightColumnOccupied() && moveDown) {
	      direction = 'down'
	      moveDown = false
	      right = false
	    } else if (right && !moveDown) {
	      direction = 'right'
	      moveDown = true
	    } else if (leftColumnOccupied() && moveDown) {
	      direction = 'down'
	      moveDown = false
	      right = true
	    } else if (!right && !moveDown) {
	      direction = 'left'
	      moveDown = true
	    }
	  }
A switch statement was then used to provide the logic for the removal and addition of the alien classes depending on the direction.

If the movement is down then depending on the current wave, more aliens will be added to the grid at the top.

Previously I had the aliens continuously moving one direction until the far right or left column were occupied. However, this ended up causing problems such as aliens completely disappearing when moving left.

### Bullets & Bombs
Both bullets and bombs utilised very similar movement logic whereby an array of arrays was used in order for functions to continue on bullets/bombs that were still in movement. It also allowed for multiple bullets/bombs to be shot at once rather than waiting for each to run it's course through the grid first.

Movement functions were also placed in a set interval and a set timeout was used in order to clear the interval when the bullet or bomb had reached the end of the grid.

Switch statement to shoot a bullet (shown previously):

	case 32: {
     bullet = ship - width
     cells[bullet].classList.add('bullet')
     bulletArrayOfArrays.push([bullet])
     bulletShot()
     bulletsShot += 1
     break
    }

Bullet movement logic:

	function bulletShot() {
	    const bulletArray = bulletArrayOfArrays[bulletsShot]
	    const interval = setInterval(() => {
	      cells[bulletArray[0]].classList.remove('bullet')
	      newBullet = bulletArray[0] - width
	      cells[newBullet].classList.add('bullet')
	      bulletArray.splice(0, 1)
	      bulletArray.push(newBullet)
	  	}, 50)
	}

The initiation of the bomb drop was slightly more complex as it incorporated randomisation, number of bombs able to be dropped at any one time and which aliens were able to drop a bomb.

Randomisation was used in order to make sure that it wasn't the same aliens dropping bombs. Additionally only the very bottom row of aliens were allowed to drop a bomb. This was done by checking that the alien who was chosen randomly to drop a bomb didn't have a cell in front of it with a className of 'alien'. 

What determined how many bombs could be dropped at any one time was defined in the wave (difficulty level). 

### Collisions

Collision logic for both bombs and bullets was incorporated into the setInterval of their movement function. An if statement was used to check if the cell which had the bullet / bomb also had the className of alien / ship. If so, then all classNames were removed and an explosion class was added for a time.

If an alien is hit by a bullet, the player gains 1 point. However, if the player's ship is hit by a bomb they will loose 1 life (out of 3). The player also looses a life if an alien reaches the bottom row.

### Game Over & Waves 

Once the player looses all their lives, the game is over and they loose 30 points. If the player manages to destroy all the aliens before they reach the bottom row, the wave is won.

If the player looses, the next game will be on the same wave level. However, if the player wins the round, they will move onto the next wave and the game will become more difficult.

Difficulty is increased by:   
1. Increased number of bombs will be dropped at once (+1).   
2. More aliens will appear (+1 row).

## Wins & Blockers  
Some of the more challenging features of the game included:   
 
* Alien movement - where aliens were dissapearing (as mentioned in Alien section above).
* Ability for multiple bullets to be fired at once.

Bug found:

- Some alien classes not being removed after collision.  
*For this I had to have a function that sweeped through the cells and remove the classes in the cells that the alien array did not include.*

- If all the aliens were removed on one side-column whilst direction was 'down', the aliens would continuously move down.


## Future Features 

**Features I added after the project week was over:**
    
-  Start page animations.   
-  Designed my own png images for aliens and bullets.

**Features I would like to implement in the future include:**

 - Saved high-score on local storage.  
 - Selection of ship image choices. 
 - Make it mobile friendly / very large screen friendly.
 
## Skills Learned

- Neat and logically sectioned code.
- What can be achieved with just the fundamentals of a few computer languages.
- By seeing other classmates versions of the game I could see that there were many different ways to achieve the same outcomes.
- That styling makes a BIG difference to the perception of an app.
