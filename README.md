# Narrative 3.5 Waypoints Mod
## Introduction
Narrative 3.5 removes waypoints to stop incompatibility with Narrative Navigation. 

This mod adds it back in with a few tweaks to allow waypoints back in the way they were.

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/8bb00dc6-1b44-4de5-a525-a8a5b3d1f6c2)

Any issues please join the Reubs discord :)


## Prerequisites
If you are using Narrative 3.5, you must have some form of HUD. I have designed this mod to work in the same way as the old waypoint system did.

Narrative 3.5. You must own the plugin. If you own Narrative 3.4 you do not need this mod.

## Install
1) Download this NarrativeUI folder which supports your engine version, and **copy and paste** it into the root of your project `/Content/` using your file explorer (not Unreal). After it's imported into Unreal you can move it inside Unreal anywhere you want.
2) Open your HUD and add in the `WBP_Waypoints` widget. (I put mine near the top so it's hidden behind other widgets)

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/a7c2e52d-030e-4d33-bfc4-8addfcb135a5)

3) Make it full-screen by selecting the widget and in the details, open the Anchors drop down, holding CTRL+SHIFT (or command on Mac) click the full-screen option.
4) Go to Graph -> Class Settings -> Implemented Interfaces -> Add `BPI_NarrativeWaypointHUD`

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/01db6273-acd8-43f6-8125-019d5558968c)

5) Open the interfaces drop-down inside MyBlueprint on your HUD and double-click the `GetWaypointsHUD` interface
6) Drag your `WBP_Waypoints` widget into the return.

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/42df9726-cbbb-4ef4-9495-cf70a467a507)

7) This project comes with the pre-modified `Go To Location` task with Waypoints so you can start using that or replace the existing one.

## Adding waypoints to new tasks
I've tried to make adding waypoints as easy as can be. For the most of it you can just copy and paste sections but I have broken it down here.

1) Open your custom task
2) Create a variable for `WaypointsUI` - type `User Widget` - Object Reference - Private

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/66cc2e9e-f232-49bb-9712-5dbae17f6f19)

3) Create a variable for `AddWaypoint?` - type `boolean`
4) Add the event `BeginTask` if it does not exist already
5) BRing in the `OwningController` variable and check if `Is Local Controller` in a branch
6) Then `GetAllWidgetsWithInterface` and set the interface to the `BPI_NarrativeWaypoints`
7) Drag from the array and choose `get (a copy)` -> IsValid -> Set your `WaypointsUI`
8) Drag in your `AddWaypoint?` variable and connect it to an `AND` node
9) Drag in the `Invert?` bool then choose `NOT`. Connect this `NOT` to the `and` node.

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/0742e423-5728-45e4-95d4-bd55b79cc023)

10) Drag from the `WaypointsUI` variable -> `GetWaypointsHUD` -> `AddWaypoint`
11) Connect `self` to the Task and leave the actor empty.

![image](https://github.com/d3kryption/narrative_3.5_waypoints_mod/assets/48034534/fd454c27-e8cd-4e4e-90b6-3b35b4c86c87)


12) Finally add the `OnTaskCompleted` and `EndTask` events
13) Drag in the `AddWaypoint?` variable and connect it to a branch (and both events)
14) Perform a validated get on the `WaypointsUI` (drag in the `WaypointsUI` variable -> right click -> convert to validated get)
15) Drag from the `WaypointsUI` -> `GetWaypointsHUD` -> `RemoveWaypoint`
16) Connect `self` to the task
17) Enjoy
