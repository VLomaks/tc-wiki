---
title: waypoint_path
description: Beginner-friendly description of flags
tags: database, master, world
published: true
editor: markdown
date: 2024-10-23T19:42:42.338Z
dateCreated: 2024-02-11T20:26:37.382Z
---

This table is used to define paths for units to follow, while [`waypoint path nodes`](/database/master/world/waypoint_path_node) is used create the points that give that path its shape.
## Structure

| Field | Type | Attributes | Key | Null | Default | Extra | Comment |
| --- | --- | --- | :---: | :---: | --- | --- | --- |
| [PathId](#pathid) | int | unsigned | PRI | NO |  |  |  |
| [MoveType](#movetype) | tinyint | unsigned |  | NO | 0 |  |  |
| [Flags](#flags) | tinyint | unsigned |  | NO | 0 |  |  |
| [Velocity](#velocity) | float |  |  | YES | NULL |  |  |
| [Comment](#comment) | varchar(255) |  |  | YES | NULL |  |  |

&nbsp;
## Description of fields

### PathId
Unique ID for each path. 
&nbsp;

### MoveType 
| Name | Value |
| ---- | ----- |
| Walk | 0 |
| Run | 1 |
| Land | 2 |
| TakeOff | 3 |

&nbsp;

### Flags
| Hex | Name |
| --- | ---- |
| 0x00 | None |
| 0x01 | FollowPathBackwardsFromEndToStart |
| 0x02 | ExactSplinePath |

When no flags are used, a unit will move directly from its last waypoint to the first. If the first and last point have the same position, the unit will move in a loop.

**FollowPathBackwardsFromEndToStart** does exactly as its name implies - at the last waypoint, the creature will turn around and follow the path back to the start.

**ExactSplinePath** applies smoothing to the path. Without it, a unit will move in a straight line from one point to the next, and turn sharply towards the next point. With ExactSplinePath enabled, the transition between points is smoothed, making the motion more organic. For example, a hexagonal loop will become a circle.
&nbsp;

### Velocity
*- no description -*
&nbsp;

### Comment
Description for the path.
| Name           | Description |
| -------------- | ----------- |
| Cosmetic Path  | Simple path linked to creature_addon |
| Cyclic Path    | Cyclic |
| Formation Path | Npc is leader of a formation |
| Scripted Path  | Npc has a scripted path |

For example: `<Npc name> - Cosmetic Path` if this path is assigned in [`creature_addon.PathId`](/database/master/world/creature_addon#pathid).
&nbsp;

## Additional Notes
One simple way to set the PathId is to take the [`entry`](/database/master/world/creature_template#entry) of the creature it belongs to, and multiply it by 10. Or by 100, if more than ten of the same creature have unique paths. These can be defined with variables in SQL:
```
SET @GUARD := 37944;
SET @PATH1 := @GUARD*10;
SET @PATH2 := @PATH1+1;
SET @PATH3 := @PATH1+2;
```
