# Soccer Pong Learning

A Unity 2D soccer-Pong project that combines paddle matches with audio-and-image learning quizzes. It includes local two-player and AI-opponent paths, player and flag selection, and separate match goals and learning rewards.

The repository contains an adaptation of a third-party Pong framework, with custom quiz, player-profile, and score workflows. This overview is based on source inspection; a fresh Unity playthrough or build has not been verified. See [validation notes](docs/VALIDATION.md).

## Project highlights

- **Matches with learning breaks:** after a goal, the goal-state script can show a quiz for the relevant human player before returning to kickoff. AI scoring can bypass the human quiz.
- **Audio-and-image questions:** questions use an audio prompt and image answer buttons. The controller handles four level values, with two choices below level 4 and four choices at level 4. Alphabet and syllable audio assets are included under `Assets/Alphabets`.
- **Learning rewards:** correct answers add five stars to the player's quiz score. Match goals and quiz stars use separate controllers.
- **Local player management:** names, identifiers, profile selection, and accumulated scores use Unity `PlayerPrefs`.
- **Match modes and feedback:** menu scripts provide local two-player and AI-opponent paths, with a kickoff countdown, goal celebrations, score displays, and winner feedback.

## Open the project

1. Clone or download the complete repository, retaining all Unity `.meta` files and asset folders.
2. Add the repository root to Unity Hub and open it with **Unity 6000.4.8f1**, as recorded in [ProjectVersion.txt](ProjectSettings/ProjectVersion.txt).
3. Allow asset imports and dependency restoration to finish. [Packages/manifest.json](Packages/manifest.json) records Universal Render Pipeline 17.4.0, Unity UI 2.0.0, Timeline 1.8.12, and the Unity 2D feature package. DOTween is included under the Pong plugins folder.
4. Open `Assets/Pong/Scenes/GamePlay.unity` and inspect the Console and Inspector references before entering Play mode.
5. Use the menu's new-game or AI-game path, select the required player names and quiz level, then use the play button for kickoff. Confirm this flow in the Editor; the UI wiring has not been verified in this review.

The package manifest also references Unity MCP from its upstream `main` branch, so that dependency is not pinned to an immutable revision. The recorded Unity version should be the starting point; compatibility with other editor versions has not been tested.

## Interaction

| Interaction | Source behavior |
| --- | --- |
| Touch or pointer drag | Human paddles use drag handlers when match input is active; two-player touch tracking is handled separately |
| Menu and profile buttons | Choose match mode, players, flags, and quiz level |
| Quiz image buttons | Select an answer and receive correct/incorrect feedback |
| Quiz sound button | Replay the answer's audio prompt |
| In-game back button | Reload the active scene to return through its startup flow |

`Paddle.cs` contains keyboard and other input helper methods, but its current `Update()` calls the drag methods directly for human players. Keyboard controls are therefore not advertised as verified gameplay controls. Simultaneous two-player input needs a touch-device test.

## Source guide

| Area | Files |
| --- | --- |
| Quiz generation, answer feedback, and reward flow | [GameController.cs](Assets/GameController.cs), [QuestionController.cs](Assets/QuestionController.cs) |
| Learning scores and local persistence | [CollectionController.cs](Assets/CollectionController.cs), [ScoreController.cs](Assets/ScoreController.cs), [ScoreP.cs](Assets/ScoreP.cs) |
| Player registration and selection | [GameStartupController.cs](Assets/GameStartupController.cs), [PlayerInputController.cs](Assets/PlayerInputController.cs), [DetailPlayer.cs](Assets/DetailPlayer.cs) |
| Match modes and human/AI paddle input | [MainMenu.cs](Assets/Pong/Scripts/UI/MainMenu.cs), [Paddle.cs](Assets/Pong/Scripts/Gameplay/Paddle.cs), [TouchChecker.cs](Assets/TouchChecker.cs) |
| Match state and score progression | [GameManager.cs](Assets/Pong/Scripts/Managers/GameManager.cs), [ScoreManager.cs](Assets/Pong/Scripts/Managers/ScoreManager.cs), [GoalState.cs](Assets/Pong/Scripts/States/GoalState.cs) |
| Winner display and score submission | [WinnerController.cs](Assets/WinnerController.cs) |

## External score service and current limits

Some profile and winner actions submit player names, identifiers, scores, and the device name to an external PHP service. The server implementation is not included in this repository, and its availability, authentication, and data handling have not been verified. This is not a verified self-contained offline release. Before a test session, inspect these call sites and use an appropriate test environment and test profiles.

The repository includes generated debug and publication artifacts. Their presence does not establish a current release or successful build. Source review did not run the Unity Editor, submit scores, or validate the serialized scene configuration.

## Attribution and reuse

The included Pong framework has source headers crediting **Skard Games** and **Cavit Baturalp Gürdin**. Its supplied [Pong Readme](Assets/Pong/Readme.pdf) and third-party assets should be consulted when reviewing provenance and reuse terms. The repository also includes DOTween and a PlayerPrefs editor utility.

This portfolio documentation does not claim authorship of the underlying framework or add a repository-wide license. Review the original framework, plugin, audio, and artwork licenses before redistribution or reuse.
