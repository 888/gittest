Design Information
==========

`1.When starting the application, a user may choose to either create a new player or log in.  For simplicity, authentication is optional.  A (unique) username will be sufficient for logging in.` .
To realize this requirement, the class “GameEntrance” was added.
Following operations were added to the class:
addPlayer(): this will create a new player
loginPlayer(): this will login to an existing player
From here, user may choose to create a new player or to login to an existing player.
No password authentication mechanism was designed.

'2.After logging in, the application shall allow players to  (1) create a word scramble, (2) choose and solve word scrambles, (3) see statistics on their created and solved word scrambles, and (4) view the player statistics.'
To realize this requirement, the class “Player” was added.
Following operations were added to the class:
createScramble(): this will create a new Scramble instance
chooseScramble(): this will choose a scramble and start a new game
showScrambleStatistics(): this will show statistics of player’s scrambles
showPlayerStatistics(): this will show player’s statistics

'3.The application shall maintain an underlying database to save persistent information across runs (e.g., word scrambles, player information, statistics).'
Local database was not shown since it does not affect the class design directly.

'4.Word scrambles and statistics will be shared with other instances of the application.  An external web service utility will be used by the application to communicate with a central server to:
a.	Add a player and ensure that their username is unique.
b.	Send new word scrambles and receive a unique identifier for them.
c.	Retrieve the list of scrambles, together with information on which player created each of them.
d.	Report a solved scramble.
e.	Retrieve the list of players and the scrambles each have solved.

You should represent this utility as a utility class that (1) is called "ExternalWebService", (2) is connected to the classes in the system that use it, and (3) explicitly list relevant methods used by those classes.  This class is provided by the system, so it should only contain what is specified here. You do not need to include any aspect of the server in your design besides this utility class.'
To realize this requirement, the utility class was represented as “<<utility>>ExternalWebService” class in the diagram.
Following operations were added to the class:
isPlayerUsernameUnique(): this will check if the desired username is unique in the system.
addPlayer(): this will add a new player to the PlayerStatistics.
loginPlayer(): this will log user into an existing player.
returnScrambleIdentifier(): this will return the unique identifier of a given scramble.
returnScrambleDetailList(): this will return the ScrambleStatistics, which contains detailed information about all scrambles.
returnPlayerDetailList(): this will return the PlayerStatistics, which contains detailed information about all players.
receiveScrambleSolved(): this will receive and mark a given scramble as “solved” in the ScrambleStatistics.

'5.When creating a new player, a user will:
a.	Enter the player’s first name.
b.	Enter the player’s last name.
c.	Enter the player’s desired username.
d.	Enter the player’s email.  
e.	Save the information.
f.	Receive the returned username, with possibly a number appended to it to ensure that it is unique.'
5.a/b/c/d
To realize this requirement, following attributes were added to the Player class:
playerFirstName: this will save player’s first name.
playerLastName: this will save player’s last name.
playerUsername: this will save player’s username.
playerEmail: this will save player’s email.
5.e/f
Following operations were added to the Player class:
addPlayer(): this will start adding a new player and allow user to input player information.
savePlayerInfo() : this will take in inputs and check with <<utility>>ExternalWebService if desired username is unique in the system. If unique then save player’s information in according attributes. If not unique, then call appendUsernameNumber() to create a unique username, then save player’s information       .
appendUsernameNumber(): this will append a random number to username.

'6.To add a word scramble, the player will:
a.	Enter a phrase (not scrambled).
b.	Enter a clue.
c.	View the phrase scrambled by the system. If the player does not like the result, they may choose for the system to re-scramble it until they are satisfied.
d.	Accept the results or return to previous steps.
e.	View the returned unique identifier for the word scramble. The scramble may not be further edited after this point.'
6.a/b
To realize this requirement, the class “Scramble” was added to the design.
Following attributes were added to the Scramble class:
initialPhrase: this will save the initial phrase which is not scrambled.
scrambleClue: this will save the clue.
6.c/d
To realize this requirement, following operations were added to the Scramble class:
generateScramble(): this will generate a scrambled version of initial phrase. It may be called for multiple times by the same player to generate different versions.
showScramble(): this will show the scramble to player.
acceptScramble(): this will let player to accept current scramble
6.e
To realize this requirement, the attribute scrambleIdentifier was added to the Scramble class. This attribute will save identifier of each scramble instances.
Following operations was added to the Scramble class:
getScrambleIdentifier(): this will send the scramble to <<utility>>ExternalWebService and show the returned identifier to player.
Following attribute was added to Scramble class:
isEditable: this will mark whether the scramble is editable.

'7.A scramble shall only mix up alphabetic characters, keeping each word together. Words are contiguous sequences of alphabetic characters separated by one or more non-alphabetic characters.'
To realize this requirement, the operation checkAlphabeticMixUp()was added to the Scramble class. This operation which will ensure that only alphabetic characters will be mixed up in the given scramble.

'8.All other characters and spacing will remain as they originally are.'
To realize this requirement, the operation checkRemainNonAlphabetic() was added to the Scramble class. This operation will ensure that all other characters and spacing will remain unchanged in the given scramble.

'9.When solving word scrambles, a player will:
a.	View the list of unsolved word scrambles, by identifier, with any in progress scrambles marked and shown first.
b.	Choose one word scramble to work on.
c.	View the scramble.
d.	Enter the letters in a different order to try to solve the scramble.
e.	Submit a solution.
f.	View whether it was correct.
g.	Return either to the puzzle, if wrong, or to the list, if correct.'
9.a
To realize this requirement, the operation getScrambleUnsolved() was added to Player class. This operation will retrieve all detailed information about all scrambles from <<utility>>ExternalWebService and show to the user only unsolved scrambles.
9.b To realize this requirement, the operation chooseScramble() was added to Player class. This will let user choose and start a scramble game.
9.c/9.d/9.e
To realize this requirement, the “CurrentGame” class was added. This class will hold all attributes and operations about the current on-going game.
Following attributes were added to CurrentGame class.
currentPlayer: this will save player username of current game.
currentScramble: this will save scramble identifier of current game.
currentSolution: will save the solution, in-progress or submitted.
Following operations were added to CurrentGame class.
showCurrentScramble(): this will let player view the scramble.
enterLetters(): this will let player input letters.
submitSolution(): this will let player submit solution and save the solution in the attribute currentSolution.
9.f
To realize this requirement, following operation was added to CurrentGame class.
checkCorrect(): this will compare the submitted solution to the scramble’s initialPhrase, and return Boolean value about whether it is correct.
9.g
To realize this requirement, following operation was added to CurrentGame class.
returnToCurrentScramble(): If the submitted solution is wrong, it will return to the current game for user to continue playing.
returnToScrambleList(): If the submitted solution is correct, the current game will end, call saveSolverInfoToScramble() and saveSolutionInfoToPlayer(). Return to the unsolved scramble list.
saveSolverInfoToScramble(): this will save solver’s information in current scramble’s attribute.
saveSolutionInfoToPlayer(): this will save scramble’s information in current player’s attribute.

'10.A player may exit any scramble in progress at any time and return to it later.  The last state of the puzzle will be preserved.'
To realize this requirement, the attribute currentSolution has been added to the CurrentGame class. will save the solution, in-progress or submitted.
Following operations were added to CurrentGame class.
saveAndExitCurrentGame(): this will save the progress in current game to the attribute currentSolution and exit game.

'11.The scramble statistics shall list all scrambles with (1) their unique identifier, (2) information on whether they were solved or created by the player, and (3) the number of times any player has solved them. This list shall be sorted by decreasing number of solutions.'
To realize this requirement, the class “ScrambleStatistics” was added. This class has a list contains detailed information about all scrambles.  All scramble instances are listed as list items.
Following attributes were added to the Scramble class:
scrambleCreator: this will save the username of who created the scramble.
scrambleSovler: this contains a list of usernames who have successfully solved the problem.
The “(1) their unique identifier, (2) information on whether they were solved or created by the player, and (3) the number of times any player has solved them” are saved in each item of ScrambleStatistics list.

'12.The player statistics will list players’ first names and last names, with (1) the number of scrambles that the player has solved, (2) the number of new scrambles created, and (3) the average number of times that the scrambles they created have been solved by other players.  It will be sorted by decreasing number of scrambles that the player has solved.'
To realize this requirement, the class “PlayerStatistics” was added. This class has a list contains detailed information about all players. All player instances are listed as list items.
Following attributes were added to the Player class:
numberScrambleSolved: this will save the number of how many scrambles have been solved by the player, update after each successful game.
numberScrambleCreated: this will save the number of how many scrambles have been created by the player, update after each scramble created.
numberTimesScreambleSolvedByOther: this will save the average number of times that the scrambles they created have been solved by other players. This number was updated after each successful game by saveSolutionInfoToPlayer() operation in CurrentGame class.
scrambleSolvedList: this will contain a list of player’s solved scrambles, update after each successful game.
“(1) the number of scrambles that the player has solved, (2) the number of new scrambles created, and (3) the average number of times that the scrambles they created have been solved by other players.” are saved in each item of PlayerStatistics list.

'13.The User Interface (UI) shall be intuitive and responsive.'
The UI was not designed in class diagram since it does not affect the class design directly.
