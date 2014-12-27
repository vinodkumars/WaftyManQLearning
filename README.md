WaftyManQLearning
=================
################################################################################
### Readme file for Automatic Flying Wafty Man
### Fall 2014 CS 221 Project
################################################################################


### Table of contents
- Introduction
- Combining code.zip and data.zip
- Quick start guide
- How to customize the scenarios
- Examples of scenarios in final report


### Introduction

Welcome to the readme for our final project! Our project is to implement
Q Learning on the Wafty Man game, which is played on the browser. The original
game was developed in HTML and Javascript by Mr.Speaker. We implemented the 
Q Learning in Javascript and HTML, and the error analysis was done in Excel
spreadsheet and Excel VBA. The beam search was performed manually, and its 
progress was tracked and analyzed in Excel.

Since Q Learning is reinforcement learning, our readme will provide you
instructions for how to set up the game to run a desired scenario. We will
provide examples to help you set up specific scenarios we described in the 
project final report.

You may view the exact code changes on GitHub:
https://github.com/vinodkumars/WaftyManQLearning

If you have not yet read the final report, we encourage you to read first.
Otherwise, it is sufficient for you to know that the current configuration
in the zip file is set up to run Wafty Man in super-fast speed, and the 
Q values are initialized with a "smart" artificial intelligence. You can open
the web console to show score statistics per 100 games.


### Combining code.zip and data.zip
In our repository, the code.zip and data.zip actually belong together.
During your setup, please extract data.zip's "log" directory and move it to
the code.zip's extracted directory:
    /WaftyManQLearning/log


### Quick start guide
- Browse to /WaftyManQLearning
- Open ./index.html in your browser
- Open web console to track scores
- RECOMMENDED, ONE TIME ONLY: enable persistent console logs
- Click on the "Play" button. It will run 1000 games in super-fast speed.
- When the 1000 games completes, it will download a file which contains the
  scores and the Q values. You can either save or discard the file.
- Refresh the webpage to restart the game


### How to customize the scenarios

Overview of the infrastructure

- The Q Learning algorithm is implemented as an extension of Mr.Speaker's
  game logic in ./src/screens/MainScreen.js. There should be no need to modify
  this file except to change the Q Learning hyper-parameters eta and epsilon.
  These hyper-parameters are already tuned and is not recommended to modify.

- We provide a control panel for the user to configure Q Learning in
  ./src/screens/QLearningData.js. This readme provides instructions on using
  this control panel.
  
- By default, Wafty-Man will prompt the user to download a log file after 
  playing 1000 games. The default filename is "QString.txt". It contains:
    - Control panel values
    - Scores per 100 games
    - Stringify'd Q values at the end of the run
  
- We have an Excel macro-enabled spreadsheet ./src/screens/heatmapm.xlsm 
  to translate stringify'd Q values from ./src/screens/QLearningData.js
  into the Excel heatmap.

- We have an Excel macro-enabled spreadsheet ./log/parselog.xlsm
  to read the game scores from "QString.txt" log files into the spreadsheet
  for additional analysis.


Overview of the Q Learning control panel (./src/screens/QLearningData.js)

- Lines 1  to 8 : Q Learning control panel implemented by global variables
- Lines 11 to 50: Stored Q values
- Lines 53 to 54: Select the desired Q values
- Lines 56 to 92: Auxilliary code. Not recommended to modify.


Example workflow: Start from no reinforcement learning,  
                  Save Q values, and
                  Start from some non-zero Q value

1.  In ./src/screens/QLearningData.js, set the values:
        doPerformQLearning      = true    // always true
        startQLearningFuture    = false   // start from no Q Learning
        heartbeatMultiplier     = (any positive number)
        maxGames                = (any positive number)
        downloadQString         = true
        muteAudio               = true or false

2.  Run Wafty Man, open the game log "QString.txt", and copy-paste the 
    stringify'd Q values in the last line to ./src/screens/QLearningData.js
    as the value of the variable storedQ*.
    
3.  Specify the value of storedQ and write helpful comment for storedQname

4.  Set the value of startQLearningFuture = true

5.  Save ./src/screens/QLearningData.js and refresh/open ./index.html


### Examples of scenarios in final report

Scenario: 100000a, 100000b, 100000c

1.  In ./src/screens/QLearningData.js, set the values:
        doPerformQLearning      = true    // always true
        startQLearningFuture    = false   // start from no Q Learning
        heartbeatMultiplier     = 10000
        maxGames                = 100000
        downloadQString         = true
        muteAudio               = true

2.  Run Wafty Man, open the game log "QString.txt", and save the game log
    into ./log/QString100000a.txt, etc. 

3.  Open ./src/screens/heatmapm.xlsm to the Control sheet, specify the 
    filepath and Q value variable name, and click "Update heat map".
    View the updated heatmap in the "Heatmap" sheet.

        Optional: duplicate "Heatmap" sheet

4.  Open ./log/parselog.xlsm to the Control sheet, specify the filepath,
    and click "Parse filepath". View updated game log in the "GameLog" sheet.

        Optional: duplicate "GameLog" sheet and update other sheets

        
Scenario: Beam search (K = 3, b = 5), starting from root node

1.  In ./src/screens/QLearningData.js, set the values:
        doPerformQLearning      = true    // always true
        startQLearningFuture    = false   // start from no Q Learning
        heartbeatMultiplier     = 10000
        maxGames                = 1000
        downloadQString         = true
        muteAudio               = true

2.  Run Wafty Man, open the game log "QString.txt", and save the game log
    into ./log/beamsearch/QStringbS_<beam search path>.txt

3.  Repeat [step 2] 4 more times (4 + 1 = 5 == b)

4.  Open ./log/parselog.xlsm to the Control sheet, specify the filepath of
    <beam search path>, and click "Parse filepath". Copy slope value from
    GameLog!F2 to "Beam Search" sheet.
    
5.  Repeat [step 4] 4 more times (4 + 1 = 5 == b)

6.  Identify the best K paths and update the <beam search path> for next level
    in "Beam Search" sheet. In next step, we do the next beam search level.

7.  In ./src/screens/QLearningData.js, set the values:
        startQLearningFuture    = true
    
    Update the stringify'd Q value of storedQbeamSearch from beam search parent
    Update the value of storedQname
    
8.  [Same as steps 2 to 6]

9.  Repeat [steps 7 to 8] until end of beam search

10. Open ./log/parselog.xlsm to the Control sheet, update beam search paths in
    cells B13:B42. For each beam search path, change value in cell B10,
    click "Parse beam search", and update other sheets.
