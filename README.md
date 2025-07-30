# Slaiju: Reborn
![Slaiju: Reborn Promo Art](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/capsula_slaiju_2.png)  

Guide the beast. Master the grid. Clear the islands from polluting cities.

Unlike its [predecessor](https://github.com/AlessandroSimeoni/Slaiju_SmashTheCity), Slaiju: Reborn wants to put the focus on the environment and pollution.  
The objective is to destroy cities responsible of polluting the world, to allow nature to breathe again.  

Originally born from a game jam project, it evolved into a full game over the course of four months, developed by a team of 12 people (5 designers, 2 programmers, 3 concept artists and 2 3D artists).  

This is my first team project outside the game academy. The game introduces a revamped graphic, new levels, an online leaderboard and a level editor.  

<a name="What-I-did"></a>
# What I did
In this project I created new grid cells and the level editor.  

The new cells are the [destructible roads](https://github.com/AlessandroSimeoni/SlaijuReborn/blob/main/Assets/Scripts/Grid/Cell/DestructibleCell.cs) ones. When the kaiju trespasses this cells, they wear down until they break and become empty cells.  

## Level Editor
The level editor allows player to create custom levels that he can play whenever he wants.  
Find the scripts [here](https://github.com/AlessandroSimeoni/SlaijuReborn/tree/main/Assets/Scripts/LevelEditor).  

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


