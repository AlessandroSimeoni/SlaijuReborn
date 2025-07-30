# Slaiju: Reborn
![Slaiju: Reborn Promo Art](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/capsula_slaiju_2.png)  

Guide the beast. Master the grid. Clear the islands from polluting cities.

Unlike its [predecessor](https://github.com/AlessandroSimeoni/Slaiju_SmashTheCity), Slaiju: Reborn wants to put the focus on the environment and pollution.  
The objective is to destroy cities responsible of polluting the world, to allow nature to breathe again.  

Originally born from a game jam project, it evolved into a full game over the course of four months, developed by a team of 12 people (5 designers, 2 programmers, 3 concept artists and 2 3D artists).  

This is my first team project outside the game academy. The game introduces a revamped graphic, new levels, an online leaderboard and a level editor.  

### ***Watch the gameplay [here](https://www.youtube.com/watch?v=QPDqdylMYKc).***
### ***Download the build [here](https://github.com/AlessandroSimeoni/SlaijuReborn/releases/tag/Slaiju_reborn_release).***
### ***Visit the Itch.io page [here](https://francescorizzoli.itch.io/slaiju-2-reborn).***

<a name="What-I-did"></a>
# What I did
In this project I created new grid cells and the level editor.  

The new cells are the [destructible roads](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/Assets/Scripts/Grid/Cell/DestructibleCell.cs) ones. When the kaiju trespasses this cells, they wear down until they break and become empty cells.  

## Level Editor
The level editor allows player to create custom levels that he can play whenever he wants.  
When [creating a custom level](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/Assets/Scripts/LevelEditor/LevelEditorNewLevelSetup.cs), the player must choose a name for the level. This name cannot be equal to another custom level's name:  

```
                foreach(CustomGrid savedGrid in customLevelSave.customGrids)
                    if (savedGrid.gridName == trimmedString)
                    {
                        createButton.interactable = false;
                        levelNameWarningTextArea.gameObject.SetActive(true);
                        levelNameWarningTextArea.text = LEVEL_NAME_NOT_AVAILABLE;
                        return;
                    }
```
And it cannot contain banned words:  

```
        private bool ContainsBannedWord(string value)
        {
            string reformedString = value.ToLower();
            reformedString = Regex.Replace(reformedString, "[^a-zA-Z]", "");

            foreach (string s in banWordsList.banList)
            {
                if (reformedString.Contains(s.ToLower()))
                {
                    createButton.interactable = false;
                    levelNameWarningTextArea.gameObject.SetActive(true);
                    levelNameWarningTextArea.text = LEVEL_NAME_BANNED;
                    return true;
                }
            }

            return false;
        }
```

The banned words are saved in a scriptable object, populated by the designers.  

During the level creation, the [LevelEditorGridComponent](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/Assets/Scripts/LevelEditor/Grid/LevelEditorGridComponent.cs) handles the logic for the selected cell toggling the cells where the player can place it.  
Player can rotate and delete the placed cells with the corresponding buttons.  

Once the grid is full the player can simulate the level by pressing the corresponding button. The `StartSimulation` method of the [LevelEditorController](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/Assets/Scripts/LevelEditor/LevelEditorController.cs) is called:  

```
        public void StartSimulation()
        {
            currentAction = null;
            uiController.ToggleSimulation(true);
            editorAudio.SetActive(false);

            simulationGrid =  cellSpawner.SpawnGrid(currentGrid);
            simulationGrid.InitializeGrid();
            ((StartCell)simulationGrid.GetCellsByID(CellID.Start)[0]).OnCharacterReturn.AddListener(StopSimulation);
            currentGrid.EnableLevelButtonsGrayBox();
            currentGrid.gameObject.SetActive(false);

            gridControls.gridComponent = simulationGrid;
            gridControls.positionController = simulationGrid.positionController;
            gridControls.transform.gameObject.SetActive(true);

            levelController.gridComponent = simulationGrid;
            levelController.positionController = simulationGrid.positionController;
            levelController.characterStateController.gameObject.SetActive(true);
            levelController.StartGame();
        }
```

Once the player is satisfied with his creation, he can save the level and play it from the main menu in the level editor section.  

Find all the scripts of the level editor [here](https://github.com/AlessandroSimeoni/SlaijuReborn/tree/main/Assets/Scripts/LevelEditor).  
