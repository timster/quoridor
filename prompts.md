# prompts

let's make a quoridor game in 1 html file. use vue3 and the composition api. start with the 9x9 grid, blue player starts at the bottom, red starts at the top.

let me enter "move mode" by clicking on blue. highlight possible moves when in move mode.

now let me place walls. horizontal walls are 2 blocks wide. vertical walls are 2 blocks tall. as long as i am not in move mode, i can hover over possible wall placements and click to place them.

blue and red get 10 walls each. keep track and add a counter above the board.

get rid of the inline styles on the walls left div, make the h2 an h1, and make consistent vertical spacing.

the walls are disappearing when i am in move mode.

when i am in move mode, click on blue exits move mode.

if i am adjacent to a wall, i should not be able to move through it. i can jump left or right instead if that's available.

make blueWallsLeft and redWallsLeft properties of playerBlue and playerRed.

this line is making wallsLeft 0.

i think isWallBetween has a bug. it only works on the first cell of the wall, if that makes sense. the second segment of a horizontal or vertical wall lets me move through it.

if blue is adjacent to red, it's offering to let me move 2 spaces away from red. that's not allowed.

i think there's a bug in this section. if i am next to red and red is next to a wall, it lets me move 2 spaces away from red.

still has the bug.

if i am next to red and red is next to the border, i am offered to move 2 spaces away from red. that is not right.

it is mostly right. sometimes when red is between blue and a wall, blue does not get offered to jump to the side.

scenario: blue is right of red. there is a wall left of red. blue is not offered to jump to the sides of red.

when blue is next to red and there are no walls anywhere, i need to be offered to jump over red

let's just redo the whole isPossibleMove function. here's the full list of rules
1. can move up down left or right as long as it's not blocked by a wall, other player, or edge of board
2. if it's blocked by other player, allow jumping 1 block over if that block is open
3. if it's blocked by another player, and jumping 1 block over would be blocked by a wall, allow jumping to the side of the other player, if that block is open

let's add win condition. if blue makes it to the top row, it's a win. if red makes it to the bottom row, it's a win. show the winner and a button to start a new game.

i don't think the player watcher for win condition is working.

let's make wall placement more robust. i should not be able to place a wall that completely blocks red path or blue path to the other side.

i want to add a simple ai for red. come up with some rules for movement/wall placing and let me review before you touch the code.

sounds good, let's try that

bugs: red does not move, and red is able to place overlapping walls.

bug: red is still placing overlapping walls.

let's start over with red ai. red ai does not place walls, it just tries to move to the other side.

bug: red ai can move on top of blue.

i don't think this overlap checking is working. first lets make sure I can't make a +.

make a new function instead of adding to canPlaceWall because canPlaceWall is used to check if it's blocking all paths. rename canPlaceWall to be more descriptive.

bug: now i can't make a T which should be allowed.

bug: now i can make a + again.

bug: i can place 2 overlapping vertical or 2 overlapping horizontal walls.

bug: still not fixed.

now when I place a wall it appears 3 blocks long.

bug: walls are still 3 blocks.

back way up and just let me place walls anywhere. we'll redo the wall intersection logic.

i think css is busted now, walls are only showing 1 block wide/tall.

make red try to jump if that would get to the finish sooner.

bug: red moves on same block as blue sometimes.

bug: sometimes red won't make a move.

bug: sometimes red won't make a move still.

let's try to fix wall placement again. i should not be able to place a wall such that a vertical wall intersects a horizontal wall. I can still make a T because that's not an intersection. I should not be able to place a horizontal wall on top of a horizontal wall or a vertical wall on top of another vertical wall.

bug: it is letting me place horizontal walls overlapping. if i have a wall at 7,3 i should not be able to place one at 7,4 since they would overlap.

bug: sometimes i can't place a horizontal wall next to a vertical wall when i should be allowed. if i have a vertical wall at 7,6 i should be able to place a horizontal wall at 7,5 but it's not allowing me.

bug: all the other wall logic works, but somehow I can place a + again.

bro i can still make a +.

all looks good, but I can STILL make a + with one vertical and one horizontal wall.

well that didn't work because i can still make a +.

bug: i can't make an upside down T. if i have a horizontal at 7,7 i can't place a vertical at 6,7 which should be allowed

bug: that doesn't work. i still can't make T but can make + again.

let's redo redAiTurn function. start simple. just move to the finish. jump over blue if it puts you closer. cannot be on the same box as blue. must move around walls like blue.

it's getting stuck becuase it's not trying to go the shortest path when blocked by a wall.

bug: now add back in the stuff where red can jump over blue.

after the 4th turn, red should sometimes try to block blue by placing walls (following all the same wall rules as blue). but don't place a wall if it would make red's path longer too.

instead of 50% make it 80% that

add instructions how to play at the bottom. include something like:
goal is to get to the other side of the board
1. on your turn, you can place a wall or move your pawn
2. walls cannot overlap or intersect
3. you cannot completely block your opponents path to the finish
4. you may jump over your oponent
5. if you cannot jump directly over, you may jump to the side
to move: click your pawn, then click where you want to move
to place wall: hover over and click where you want the wall

replace all styles with tailwind

you got rid of the "how to place a wall" and "how to move"
