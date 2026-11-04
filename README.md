# ReZeN-Raycast-Projectile

Raycast-based projectiles for Roblox: server simulation, client rendering,
damage, hit/expire callbacks, pierce, hit bindings and a shared explosion effect.

## CONTENTS

- ReplicatedStorage.Shared.ProjectileSystem — projectile module
- ReplicatedStorage.Shared.ExplosionEffect — explosion module
- ReplicatedStorage.Templates.Projectiles.ProjectileDef — projectile template
- ReplicatedStorage.Templates.Explosions.Explosion — explosion template
- ServerScriptService.ProjectileRunner — keeps the simulation alive
- Workspace (you can delete this) / StarterPlayerScripts (you can delete this) — demo

## USAGE

Start once:
	local ProjectileSystem = require(game.ReplicatedStorage.Shared.ProjectileSystem)
	ProjectileSystem.start()

Fire:
	local projectile = ProjectileSystem.fire({
		template = game.ReplicatedStorage.Templates.Projectiles.ProjectileDef,
		muzzle = gun.Body.FireProj,
		speed = 100,
		gravity = Vector3.new(0, -100, 0),
		ignore = { gun },
		damage = 25,
		onHit = function(projectile, result)
			print("hit", result.Instance, result.Position)
		end,
	})

Fire options:

	template          Instance    required, cloned for every shot
	origin            CFrame | Vector3 | Attachment | BasePart | Model
	muzzle            Attachment  used when origin is nil
	direction         Vector3     defaults to the muzzle / look direction
	speed             number      default 100
	simulationRate    number      default 120
	gravity           number | Vector3  default workspace gravity
	acceleration      Vector3     extra constant acceleration
	orientation       CFrame      extra rotation
	alignToVelocity   boolean     default true
	parent            Instance    default workspace.Projectiles
	name              string
	ignore            { Instance }
	ignoreWater       boolean     default true
	respectCanCollide boolean     default true
	collisionGroup    string
	lifetime          number      default 8, 0 = forever
	maxHits           number      default 1
	pierce            number      maxHits = pierce + 1
	destroyOnHit      boolean     default true
	damage            number      applied to the nearest Humanoid
	onHit             function(projectile, result)
	onExpire          function(projectile)
	hitBindings       { { target, callback } }

Events:

	projectile.hit:Connect(function(projectile, result) end)
	projectile.expired:Connect(function(projectile) end)
	projectile.stopped:Connect(function(projectile) end)
	projectile.destroyed:Connect(function(projectile) end)
	projectile:bindHit(target, function(projectile, result) end)
	projectile:unbindHit(target)

Projectile API:

	:getPosition()  :getCFrame()  :getVelocity()  :getInstance()
	:isAlive()      :setVelocity(v)  :stop()  :destroy()

## TEMPLATE USAGE

Projectile template:
	Any BasePart or Model. Every BasePart inside is anchored and made
	non-collidable, non-touchable and non-queryable automatically.
	For a Model set PrimaryPart — it is used for PivotTo.
	The gun and the shooter go into "ignore".

Muzzle:
	An Attachment on the barrel (for example Body.FireProj), passed as
	"muzzle". Direction defaults to the parent part LookVector.

Explosion template:
	ReplicatedStorage.Templates.Explosions.Explosion must be a Model with:
		Explosion          MeshPart (fireball)
		ExplosionShockWave MeshPart (flat ring)
		Sound inside Explosion (optional)

	local ExplosionEffect = require(game.ReplicatedStorage.Shared.ExplosionEffect)
	ExplosionEffect.play(result.Position)
	ExplosionEffect.play(position, { scale = 1.5 })

## CONTACT

Discord: petrensky2523
Roblox: ReZeN_DevS
