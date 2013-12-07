********************************************************************************
*                                                                              *
*              Advanced spawning mod (adv_spawning) 0.0.2                      *
*                                                                              *
*     URL: http://github.com/sapier/adv_spawning                               *
*     Author: sapier                                                           *
*                                                                              *
********************************************************************************


Description:
--------------------
Advances spawning mod is designed to provide a feature rich yet easy to use
spawner for entites. It's purpose is to support spawning within large numbers
of different environments. While adv_spawning supports a wide configurable range
of spawning situations, it's performance impact is clamped at minimal level.
To achieve this performance goal adv_spawning is intended for low frequency
entity spawning only. Typical spawn rate will be a low single digit count of
entities per second throughout whole world.


API:
--------------------
adv_spawning.register(spawner_name,spawning_def) --> successfull true/false
^ register a spawn to adv_spawning mechanisms
^ spawner_name a unique spawner name
^ spawning_def is configuration of spawner

adv_spawning.statistics() --> statistics data about spawning

Spawning definition:
--------------------
{
	spawnee = "some_mod:entity_name", -- name of entity to spawn OR function to be called e.g. func(pos)
	absolute_height =     -- absolute y value to check
	{
		min = 1,          -- minimum height to spawn at
		max = 5           -- maximum height to spawn at
	}

	relative_height =     --relative y value to next non environment node
	{
		min = 1,          -- minimum height above non environment node
		max = 5           -- maximum height above non environment node
	}

	spawn_inside,         -- [MANDATORY] list of nodes to to spawn within (see spawn inside example)
	surfaces,             -- list of nodes to spawn uppon (same format as spawn_inside)

	entities_around =     -- list of surrounding entity definitions
	{
		entity_def_1,
		entity_def_2,
		...
	},

	nodes_around =        -- list of surrounding node definitions
	{
		node_def_1,
		node_def_2,
		...
	},

	light_around =        -- list of light around definitions
	{
		light_around_def_1,
		light_around_def_2,
		...
	},

	humidity_around =     -- list of humidity around definitions
	{
		humidity_around_def_1,
		humidity_around_def_2,
		...
	},

	temperature_around =  -- list of temperature around definitions
	{
		temperature_around_def_1,
		temperature_around_def_2,
		...
	},

	mapgen =                 -- configuration for initial mapgen spawning
	{
		enabled = true,      -- mapgen spawning enabled or not
		retries = 5,         -- number of tries to spawn a entity prior giving up
		spawntotal = 3,      -- number of entities to try on mapgen
	},


	flat_area =              -- check for amount of flat area around,
							 -- (only usefull for ground bound mobs)
	{
		range = 3,           -- range to be checked for flattness
		deviation = 2,       -- maximum number of nodes not matching flat check
	},

	collisionbox = {},       -- collisionbox of entity to spawn (usually same as used for entiy itself)
	spawn_interval = 200,    -- [MANDATORY] interval to try to spawn a entity
	custom_check = fct(pos), -- a custom check to be called return true for pass, false for not pass
	cyclic_spawning = true   -- spawn per spawner step (defaults to true)
}

Light around definition:
{
	type = "TIMED_MIN",       -- type of light around check
							  -- TIMED_MIN/TIMED_MAX at least this light level at specified time within whole distance
							  -- OVERALL_MAX,OVERALL_MIN at least this light level at any time
							  -- CURRENT_MIN,CURRENT_MAX at least this light level now
	distance = 2,             -- distance to check (be carefull high distances may cause lag)
							  -- WARNING: light check is a very very heavy operation don't use large distances
	threashold = 2,           -- value to match at max/at least to pass this check
	time = 6000               -- time to do check at (TIMED_MIN/TIMED_MAX only)
}

Surrounding node definition:
{
	type = "MIN",             -- type of surround check valid types are MIN and MAX
	name = { "default:tree" },-- name(s) of node(s) to check
	distance = 7,             -- distance to look for node
	threshold = 1             -- number to match at max/at least to pass this check
}

Surrounding entity definition:
{
	type = "MIN",              -- type of surround check valid types are MIN and MAX
	entityname = "mod:entity", -- name of entity to check (nil to match all)
	distance = 3,              -- distance to look for this entity
	threshold = 2              -- number to match at max/at least to pass this check
}

Surrounding temperature definition:
{
	type = "MIN",              -- type of surround check valid types are MIN and MAX
	distance = 3,              -- distance to look for this temperature
	threshold = 2              -- number to match at max/at least to pass this check
}

Surrounding humidity definition:
{
	type = "MIN",              -- type of surround check valid types are MIN and MAX
	distance = 3,              -- distance to look for this humidity
	threshold = 2              -- number to match at max/at least to pass this check
}

spawn_inside definition (list of nodenames):
{
	"air",
	"default:water_source",
	"default:water_flowing"
}

Statistics:
{
	session =
	{
		spawners_created = 0,   -- number of spawners created this session
		entities_created = 0,   -- number of spawns done
		steps = 0,              -- number of steps
	},
	step =
	{
		min = 0,                -- minimum time required for a single step
		max = 0,                -- maximum time required for a single step
		last = 0,               -- last steps time
	},
	load =
	{
		min = 0,                -- minimum load caused
		max = 0,                -- maximum load caused
		cur = 0,                -- load caused in last step
		avg = 0                 -- average load caused
	}
}