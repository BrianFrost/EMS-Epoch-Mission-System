## Epoch Mission System [EMS]
### Version 0.2.6 [WIP] Work in Progress

_Credits: Lazyink, TheSzerdi, Falcyn , TAWTonic, EPOCH DEV's<br>
Merged DayZ Chernarus Mission System's from TheSzerdi and Lazyink with permission._


Epoch Mission System ( EMS ) is a modified version of Lazyink & TheSzerdi's DayZ Chernarus Mission systems. The two projects were merged by Fuchs and released with some SargeAI configs to enhance the gameplay of Epoch Chernarus servers. The project was then renamed to "Epoch Mission System" after MimiC joined Fuchs and further development was done on it. It has now been released as a new project, with a total of 26 unique missions that are specific to Epoch servers.
The Sarge AI config has been removed.

_This mission system will not work properly on non-Epoch servers or servers that do not have DZAI already installed._

**Requirements:**

* Notepad++
* PBO Manager
* Epoch 1.0.3.1 Server
* DZAI 

**Difficulty**

Easy : 10-15 minutes

* Some knowledge of Epoch Server & Mission file locations
* How to use PBO manager to unpack and pack PBO files

===========================================================================

## Installation Instructions
### dayz_server PBO Instructions

Download and unpack the most recent release of EMS from <a href="https://github.com/TheFuchs/Epoch-Mission-System--EMS-/releases">our GitHub Release Section</a>. Currently this is version 0.2.5 with hotfixes from 26.12.2013 

Make a copy of your <b>dayz_server.pbo</b> and rename it <b>dayz_server.pbo.bak</b>

Unpack your <b>dayz_server.pbo</b> to a folder called <b>dayz_server</b>

Copy the <b>Missions</b> folder from the EMS download ,paste and place the contents in the existing since Patch 1.0.3.1 missions folder <b>dayz_server</b>


<b>Edit your server_functions.sqf</b><br>Located: dayz_server\init\server_functions.sqf<br>

<b>Around line 30 look for this:</b>

    server_deaths = compile preprocessFileLineNumbers "\z\addons\dayz_server\compile\server_playerDeaths.sqf";

<b>Add this after it:</b>

    fnc_hTime = compile preprocessFile "\z\addons\dayz_server\EMS\misc\fnc_hTime.sqf"; //Random integer selector for mission wait time

<b>Around line 540 look for this:</b>
	

    dayz_recordLogin = {
      private["_key"];
      _key = format["CHILD:103:%1:%2:%3:",_this select 0,_this select 1,_this select 2];
      _key call server_hiveWrite;
    };


<b>Insert this after it:</b>
	

    //----------InitMissions--------//
    MissionGo = 0;
    MissionGoMinor = 0;
    if (isServer) then { 
      SMarray = ["SM1","SM2","SM3","SM4","SM5","SM6","SM7","SM8","SM9","SM10","SM11","SM12","SM13"];
      [] execVM "\z\addons\dayz_server\EMS\major\SMfinder.sqf"; //Starts major mission system
      SMarray2 = ["SM1","SM2","SM3","SM4","SM5","SM6","SM7","SM8","SM9","SM10","SM11","SM12","SM13"];
      [] execVM "\z\addons\dayz_server\EMS\minor\SMfinder.sqf"; //Starts minor mission system
    };
    //---------EndInitMissions------//

	
<b>Edit server_updateObject.sqf</b><br>Located: dayz_server\compile\server_updateObject.sqf

<b>Around line 22 look for this:</b>

    { 
      diag_log(format["Non-string Object: ID %1 UID %2", _objectID, _uid]);
      //force fail
      _objectID = "0";
      _uid = "0";
    };

<b>Insert this after it:</b>

    if (_object getVariable "Sarge" == 1) exitWith {};

    if (!_parachuteWest and !(locked _object)) then {
    //if (_objectID == "0" && _uid == "0") then
    if (_objectID == "0" && _uid == "0" && (vehicle _object getVariable ["Sarge",0] != 1)) then

<b>Edit server_cleanup.fsm</b><br>Located: dayz_server\system\server_cleanup.fsm

<b>Around line 298 look for this:</b>

    if(vehicle _x != _x && !(vehicle _x in PVDZE_serverObjectMonitor) && (isPlayer _x) && !((typeOf vehicle _x) in DZE_safeVehicle)) then {" \n

<b>Replace with this:</b>

    if(vehicle _x != _x && !(vehicle _x in PVDZE_serverObjectMonitor) && (isPlayer _x) && (vehicle _x getVariable [""Sarge"",0] != 1) && !((typeOf vehicle _x) in DZE_safeVehicle)) then {" \n

<b>Repack your dayz_server.pbo and replace your original one.</b>

=========================

### Mission PBO Instructions

Unpack your mission PBO file using PBO Manager into a folder

Copy the <b>debug</b> folder from the EMS download to the root of your mission folder

<b>Edit your init.sqf file</b>

At the very end of you <b>init.sqf</b> file paste the following block of code:

    // Mission System Markers
    [] execVM "debug\addmarkers.sqf";
    [] execVM "debug\addmarkers75.sqf";

This will make the mission markers show up on the map for players that have died and respawn, or connect to the server after a mission has already spawned.

<b>Repack your mission PBO using PBO Manager and replace your existing _mission_.pbo file</b>

<b>Next year follows the integration of DayZ Bandit AI ,the current AI will be removed then</b>
