chessgame

I still have one bug. when the game run on mydesktop , it work fine.
 but when i change to laptop. move handler of the gui doesn't work well
i will fix it soon

I have spend a lot of time working on this game.

Main class
	game start from main class
	main class will initialize player class ,gui class and the board class.

Board class .
	the board class contain squares, gui ,two player and move
	the main function is initialize square and put piece of corresponding square.
	the board class will handle input from user input(Gui)
	and pass the request to move class,let the move class response to gui class

Gui class
	Gui class will response all user request to board class
	its instance member jbutton is similar to square in the board class
	when i pass square and piece to gui, i will let the jbutton perform move piece
	and clear piece 
	all of the ui setting will be done in this class

Move class
	the main function is to determine whether player could move piece on the square
	it is the place where i deal with game logic

Piece class
	it has instance member
	--x x coordiante of the piece
	--y y coordinate of the piece
	--color mark the owner
	--pieceIcon the piece of the piece
	it has method like setIcon,all of the subclass will implement this method individually



Player class
	different color for different player
	it contain piece as instance member
	it also has method
	--initializepiece(), i will put the corresponding x ,y coordinate on the piece

Square class
	it has its x,y coordinate and piece as instance member.
	it also has method like 
	--occupySpot()(put piece on the square)
	--releasespot()(clear piece on the square)
	--isOccupied(check if a piece on the square)

Bishop,King,Knight,Pawn,Queen,Rook
	all of the subclass has one constructor
	it will setup their individual x coordinate,y coordinate, and color,and pieceIcon
=====================================================================================================================
2019-02-16
below is my square pics
			N
		E		W
            S		
 * square pics
 * (0,0)- - - - - - - -(7,0)x
 *      - 1 2 3 4 5 6 7
 *      - 2 
 *      - 3
 *      - 4
 *      - 5
 *      - 6
 * (7,0)- 7            -(7,7)
 *      y
 - stand for piece 


1 save and load feature
	when save ,serialize board,player,square,and piece,and gui
	when load,I reset the gui.
	for gui, i clear all of the piece icon on the jbutton.
	for Board,I reset all of the piece with square and put them on the gui again.

2 change turn
	Move class contain turn as instance member initialize as turn
	when black player move his piece, change turn to white player

3 piece movement
	3.0 Piece
		when move piece ,the move class call validMove method of the piece.
		base on the type of piece ,each piece will override validmove individually

		3.0.1 move_Hor_Ver()
			This method shard by rook and Queen
			First, check whether piece is move horizontal or vertical
			Second, call checkXdirection and checkYdirection to get the specific direction of movement
			Third, call isNoBlock check if other piece block in the way
	
		3.0.2 moveDiagonal()
			This method shard by bishop and Queen
			First, check whether piece is move Diagonal
			Second, call checkXdirection and checkYdirection to get the specific direction of movement
			Third, call isNoBlock check if other piece block in the way
	
		3.0.3 isNoBlock()
			share by Knight and Queen and bishop
			as long as destY and destX is not near curY and curX
			move curX or curY one step toward destX or destY
			once any piece is in its way ,return false otherwise turn true
	
		3.0.4 checkYdirection()
			return the directio based on piece move on the board
			if curY>destY, piece move to north
			if curY<destY, piece move to south
			if curY=destY, piece no move
	
		3.0.5 checkXdirection()
			return the directio based on piece move on the board
			if curX>destX, piece move to east
			if curX<destX, piece move to west
			if curX=destX, piece no move


	3.1 pawn 鈥� can move forward 1 or 2 on that piece鈥檚 FIRST move, 1 forward afterwards. 
		As white and black pawn has different direction. checkYdirection for black pawn(1) and white pawn(-1)
		for the first move , pawn can move two step. so need to check if there is any piece block its way
		after first move ,1 forward afterward, as long as the direction if correct.
	
	3.2 Castle/Rook 鈥� can move horizontal or vertical unlimited number of spaces as long as the way is clear of other pieces
		this method shared by rook and queen, so it is in piece class
		First,call move_Hor_Ver() method 
		second,call checkXdirection and checkYdirection to get specific direction
		third, call isNoBlock() to check if other piece block its way


	3.3 Bishop 鈥� can move on the diagonal unlimited number of spaces as long as the way is clear of other pieces
		this method shared by bishop and queen, so it is in piece class
		First,call moveDiagonal method to check if it is move diagonal
		second,call checkXdirection and checkYdirection to get specific direction
		third, call isNoBlock() to check if other piece block its way
	
	3.4 Knight 鈥� moves in an 鈥淟鈥� pattern of 2-1 or 1-2 in any direction, the path does not need to be clear
	
		Math.abs(destX - curX)    Math.abs(destY - curY)
				2							1
				1							2
		the above move is valid 
	3.5 Queen 鈥� 
		can move in any direction unlimited number of spaces as long as the way is clear of other pieces
		Queen move like Rook and Bishop
	
	3.6 King 鈥� 
		can move in any direction 1 space
		As king can move in any direction 1 space
		As long as Math.abs(destX - curX) is 0 or 1 and Math.abs(destY - curY) is 0 or 1 , the move is valid
		Math.abs(destX - curX) Math.abs(destY - curY)     direction
				0						0					no move
				0						1				 	move vertical
				1						1					move diagonal 		
				1						0					move horizontal
=====================================================================================================================		
Asign 2c March 17,2019

3d board layout
board[0] board[1] board[2]

1.Gui class
	1.1gui layout 
		1.1.1 for regular board ,the layout is 
			myJpanel >jtbnArr(1:1)

		1.1.2 for 3d chessboard, the layout is
			myJpanel>mySJPanels(1:3)	
			mySJPanels[i]>jbtn3DArr[i](1:1)
			
			mySJPanels>mySJPanels[i](1:3)	
			jbtn3DArr >jbtn3DArr[i](1:3)
	
		1.1.3 for regular board , i use 2d jbtnArr,
			  for 3d chessboard I use a 3d jbtnArr for each board	
	
		1.1.4 I will set each jbtn a boardNo(0,1,2).each jbtn on the same jbtnArr has the same boardNo.
			when i click on the jbtn on board[0],the gui know this jbtn belong board[0]
	
	1.2.movepiece method
		orginal i use one method(movePiece)  move piece on gui
		now i use two method(setPiece and clearPiece) individually 
		because in 3d board, you might move piece between different board

2 board init
	2.1 chessboard init
		2.1.1chessboard constructor
			1 initialize gui,player,squares,movehandler by Board constructor
			2 initialSquare();//give each square x,y position 
			3 set chessboard is curBoard
		2.1.2 Main class
			4 initPieceOnBoard()// put piece on curboard


	2.2 3d chessboard init
		2.2.1 3d chessboard constructor
			1. initialize gui,player,squares,movehandler by Board constructor
			2. init 3 chessboard //see above chessboard init
			3. give each board an unique boardNo(0,1,2)
			4. reset curBoard to board[0]
		2.2.2 Main class
			5. initPieceOnBoard()// put piece on curboard

3 Board class
	3.1 instance member
		every board has gui,players,squares,movehandler.
		it also has a boardNo,and curBoard(used to initialize piece on board)
	3.2 constructor //initialize gui,player,squares,movehandler
	3.3 other method
		movepiece()
		other setter and getter method

4 chessboard class
	4.1 constructor
	4.2 other method
		initialSquare()
		initPieceOnBoard()
		resetBoard()
		// as these above method only for chessboard,put it under chessboard class
		movepiece();

5 chessboard3d class
	5.1 instance member
			chessboard[] board// board will have 3 chessboard
	5.2 constructor
	5.3 movePiece();
	
6 Square class
	when chessboard init square, it will pass the chessboard object into square.
	so each square know which chessboard it belong to.

7 Main Class
	if use option 1:
	3d chessboard init
	if use option 2:
	regular chessboard init

8 Piece class
	if it move on the same board , the same as regular chess board move
	if it move on different board, first you have to get on different board first,
	then you could move like on the same board
	i think the basic idea is you have to get to the board then move.
	and if you would like to move up or down one board, you move one step.
	if move up or down two board ,you move two step

9 Pawn
	for the first move, pawn can move two step. so it could move up two board
	after that, pawn can move one step. so it could move up or down one board if you like 
10 Rook
	although rook can move unlimited step,
	if it need move up or down two board, it should move two step vertical up or down(in denis demo)
	if it need move up or down one board ,it should move one step vertical up or down.
11 Knight
	in dennis demo, knight can move up or down one board or two board in 2:1 move or 1:2 move
	i think i did not change the code too much here.
12 Bishop
	although Bishop can move unlimited step,
	if it need move up or down two board, it should move two step diagonal up or down(in denis demo)
	if it need move up or down one board ,it should move one step diagonal up or down.
13 Queen
	move like rook and bishop
14 King
	as king can only move in any direction one step.
	it will only move up or down one board.(in dennis demo)

15 regarding join paths 
	I use '/' instead of '\\' .
	let me know if the problem still exist

===================================================================================================================
