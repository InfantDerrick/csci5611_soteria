# CSCI 5611 Final Project

- Infant Derrick Gnana Susairaj (gnana014)
- Kailash Kalyanasundaram (kalya023)

## Project Description

For the Final Project:

We decided to create a Interactive Game called Σωτηρία (Soteria).  We love playing competitive FPS games, like COD and CSGO, so we wanted to create our own FPS game using topics we covered from class this semester. We created Soteria, where the user navigates through our terrain protecting a crystal from zombies at night. To assist with survival, we created a shield mechanism with blocks using Collision Detection and Rigid Bodies to block the zombies out like in Minecraft. We implemented ray tracing to simulate bullet mechanics, which helps the user kill zombies and survive longer. The zombies will start following the user based on the user's proximity using our pathfinding algorithm. The goal of the game is to protect the crystal and stay alive as long as possible. 

### Key Algorithms and Approaches


#### Build Using Power
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityStandardAssets.Characters.FirstPerson;

public class  Build : MonoBehaviour
{
    [SerializeField] Camera fpsCamera;
    [SerializeField] RigidbodyFirstPersonController fpsController;
    public GameObject obs;
    private void OnDisable()
    {
        
    }

    private void Update()
    {
        if (Input.GetMouseButtonDown(1))
        {
          Build(Input.mousePosition);
        }
    }
    public void PutCoin(Vector2 mousePosition)
    {
        RaycastHit hit = RayFromCamera(mousePosition, 1000.0f);
        GameObject.Instantiate(coinPrefab, hit.point, Quaternion.identity);
    }

    public RaycastHit RayFromCamera(Vector3 mousePosition, float rayLength)
    {
         RaycastHit hit;
         Ray ray = Camera.main.ScreenPointToRay(mousePosition);
         Physics.Raycast(ray, out hit, rayLength);
         return hit;
    }
}
```
#### OpTargeting
```c#
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;

public class OpTargeting : MonoBehaviour
{
    [SerializeField] float chaseRange = 5f;
    [SerializeField] float turnSpeed = 5f;

    NavMeshAgent navMeshAgent;
    float distanceToTarget = Mathf.Infinity;
    bool isProvoked = false;
    EnemyHealth health;
    Transform target;

    void Start()
    {
        navMeshAgent = GetComponent<NavMeshAgent>();
        health = GetComponent<EnemyHealth>();
        target = FindObjectOfType<PlayerHealth>().transform;
    }

    void Update()
    {
        if (health.IsDead())
        {
            enabled = false;
            navMeshAgent.enabled = false;
        }
        distanceToTarget = Vector3.Distance(target.position, transform.position);
        if (isProvoked)
        {
            EngageTarget();
        }
        else if (distanceToTarget <= chaseRange)
        {
            isProvoked = true;
        }
    }

    public void OnDamageTaken()
    {
        isProvoked = true;
    }

    private void EngageTarget()
    {
        FaceTarget();
        if (distanceToTarget >= navMeshAgent.stoppingDistance)
        {
            ChaseTarget();
        }

        if (distanceToTarget <= navMeshAgent.stoppingDistance)
        {
            AttackTarget();
        }
    }

    private void ChaseTarget()
    {
        GetComponent<Animator>().SetBool("attack", false);
        GetComponent<Animator>().SetTrigger("move");
        navMeshAgent.SetDestination(target.position);
    }

    private void AttackTarget()
    {
        GetComponent<Animator>().SetBool("attack", true);
    }

    private void FaceTarget()
    {
        Vector3 direction = (target.position - transform.position).normalized;
        Quaternion lookRotation = Quaternion.LookRotation(new Vector3(direction.x, 0, direction.z));
        transform.rotation = Quaternion.Slerp(transform.rotation, lookRotation, Time.deltaTime * turnSpeed);
    }

    void OnDrawGizmosSelected()
    {
        Gizmos.color = Color.red;
        Gizmos.DrawWireSphere(transform.position, chaseRange);
    }
}
```
#### Terrain Generation using noise
```c#
public double noise(double nx, double ny) {
  // Rescale from -1.0:+1.0 to 0.0:1.0
  return gen.GetValue(nx, ny, 0) / 2.0 + 0.5;
}

double value[height][width];
for (int y = 0; y < height; y++) {
  for (int x = 0; x < width; x++) {
    double nx = x/width - 0.5, 
           ny = y/height - 0.5;
    value[y][x] = noise(nx, ny);
  }
 }
```

For our project's success, we had to use a multitude of things we learned in class.
- RigidBody Simulation
- Collision Detection
- Path Finding
- Ray Tracing
- Terrain Generation using noise

I think the limiting factor when scaling this project would be the way that we implemented our opponents in this game. Currently how it is set is there are a limited amount of opponents pregenerated that are just rendered and de-rendered based on the location of the player is on. If the map parameters were set to be bigger than they are right now, then we would need to create way more enemies which would increase the size of the projects unreasonabley. If we were to proceed with this and try to deploy it where the enemies are spwaned in dynamically, the performance would not dip regardless of the size of them map. We were able to get the generation of the blocks that are generated to be dynamic but with the opponent having so many coponents and changes and animation, it was hard for us to deploy an opponent that was competent and followed all the rules of the game. 

### State-of-the-Art Approaches

During the related work search, we came across many papers about collision detection and planning in games. The two main state-of-the-art techniques we researched were bounding box collision detection and collision detection using air meshes. Implementing collision detection with air meshes in Unity was too challenging for what we wanted to do in Soteria. Furthermore, we wanted to make an intuitive building schema to protect the user from the zombies. To do this, we used the state-of-the-art bounding box collision detection techniques in terms of actual building blocks to test it. Unfortunately, we didn't have enough time to make it more realistic so we just used building blocks like in Minecraft to show the bounding box collsion detection technique during the game. These boxes are built by the user to protect the crystal and the user from the zombies. The zombies can't get through the boxes and reroutes to the user in another way instead because of the bounding box collision detection technique. Therefore, the related work research was very helpful for us because we ended up using a very similar approach to the bounding box approach that we researched for the shielding mechanism in our game.

### Feedback Discussion

During the progress report, we received a lot of positive feedback about how our environment looks and the overall 3D movement and rendering. However, a common suggestion for our project was to create enemies to shoot and kill so that the user has something to shoot at. In our final version, we added the zombies from the Unity Store as our targets and integrated our pathfinding algorithm and ray tracing to interact with the user. Therefore, in our final version, we successfully added enemies and other features like the shielding blocks, crystal, and ray tracing to make the overall game smoother and a lot more complete compared to the progress report. The feedback definitely helped us take the game into a better direction to create a better final version of Soteria.

### Evolution of Project

While planning this project out, we intended for the building mechanics to be used in terms of a fluid particle simulation so that it can effectively block the enemies from the user. However, while implementing the game, we ended up pivoting to a rigid body simulation of simple blocks to defend the user. The enemies were supposed to be particle systems, but instead we changed to zombies. Finally, we meant for the user to use magic projectiles to attack the enemy and defend the crystal, but we didn't have time to enhance the physics of the magic so we chose to switch to use a gun with ray tracing instead.


### Future Work

There are some directions Soteria can be extended to make the interactive game even better. We can create the enemies/zombies dynamically and make sure each one is unique so that it is scaleable as more time passes and the enemies act differently so the user has to be more strategic. Also, as I mentioned earlier, the shielding mechanism is just building blocks currently so we would want that to be more fluid obstacles that block the zombies/enemies. Additional features that can be implemented are multiple maps/environments that the user can play in and also, include some different powerups to help the user survive. For instance, we can have a slow down power up that slows down the zombies.

## Media

[Main Final Project Demo]( https://youtu.be/msnqzk_xdiY).
