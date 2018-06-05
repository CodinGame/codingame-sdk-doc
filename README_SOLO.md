# Introduction

This document will show you what differs from a Multiplayer game to create a **Solo** or an **Optimization** game. You can read the main instructions [here](https://github.com/CodinGame/codingame-sdk-doc).

Note that an **Optimization** game *is* a **Solo** game.

## Solo game specific classes

In order to create a **Solo** game, you will need to use a few specific classes.
- Your `Player` class must extend from `AbstractSoloPlayer`
- Your `Referee` class must inject (through Guice) an instance of `SoloGameManager<Player>`

[More info about these two classes](https://github.com/CodinGame/codingame-game-engine/tree/master/engine/core)

You will also need to use an instance of `SoloGameRunner` to run your game locally. [More info on the Game Runner](https://github.com/CodinGame/codingame-game-engine/tree/master/runner/#running-a-solo-game)

## Project Hierarchy

Here's an example of the file hierarchy for a **Solo** game project:
```
.
├── config
│   ├── config.ini
│   └── statement_en.html
│   └── test1.json
│   └── test2.json
├── pom.xml
├── README.md
├── src
│   ├── main
│   │   ├── java
│   │   │   └── com
│   │   │       └── codingame
│   │   │           └── game
│   │   │               ├── Player.java
│   │   │               └── Referee.java
│   │   └── resources
│   │       └── view
│   │           ├── assets
│   │           │   └── Background.jpg
│   │           └── config.js
│   └── test
│       ├── java
│       │   ├── Main.java
│       │   ├── Player1.java
│       └── resources
│           └── log4j2.properties
```

- **./config:** contains the settings such as the game title, statement, test cases, etc.
- **./src/main/java:** source code of the game itself
- **./src/main/resources:** graphical assets and configuration of the view
- **./src/test/java:** classes used for local development (code to test your game)

## Test cases

The test cases are json files and must be placed in the `config` directory. See [How to create a test case file](https://github.com/CodinGame/codingame-game-engine/tree/master/runner/#test-case-file)

## Several questions

Just like a Multiplayer game, a **Solo** game can have several questions (these are called *leagues* in a Multiplayer game). Having multiple questions can help segment your game into parts of rising difficulty.

In order to create several questions, you must create sub directories named `level<number>` where the `<number>` will be used to display your questions in the right order. These directories can contain the same kind of files as your `config` directory, such as a statement, test cases, etc. 

Note that you **do not need** to have copies of your files in these sub directories if they are common to all the questions. You can, for instance, have one `config.ini` file in your `config` directory that will be inherited by all the questions.
However, take in consideration that **Optimization** games do **not** support this feature.

## End of the game

In **Solo** games, you do not compete against other players. Therefore, you will need to decide when a player *wins* or *loses*. To do that, you have two methods:
```java
gameManager.winGame();
```
and
```java
gameManager.loseGame();
```
Which can take an optional `String` parameter as an end game message.

## Optimization game

**Optimization** games are **Solo** games with additional configuration.

First, you will need to add a `criteria` and a `sorting_order` property in `config.ini`.
- The `criteria` corresponds to the label of the player's score. For example, it can be `Points`, `Fuel` or `Distance`.
- The `sorting_order` determines the ranking order. Its value must be either `asc` or `desc`. If the player whose Fuel quantity is higher should be first, choose `asc`. If the goal is to win the game in the shortest time, choose `desc`.

You can also choose to translate your criteria in French by using the optional properties `criteria_en` and `criteria_fr`.

Once this configuration is done, you can now set your player's score. We advise you to set it at the end of the game as below:
```java
@Override
public void onEnd() {
    // I set my criteria "Fuel" to what's left of the player's fuel
    gameManager.putMetadata("Fuel", remainingFuel);
}
```
