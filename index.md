# CSCI 5611 Final Project

- Infant Derrick Gnana Susairaj (gnana014)
- Kailash Kalyanasundaram (kalya023)

## Project Description

For the Final Project:

We decided to create a Interactive Game called Σωτηρία (Soteria).  We love playing competitive FPS games, like COD and CSGO, so we wanted to create our own FPS game using topics we covered from class this semester. We created Soteria, where the user navigates through our terrain protecting a crystal from zombies at night. To assist with survival, we created a shield mechanism with blocks using Collision Detection and Rigid Bodies to block the zombies out like in Minecraft. We implemented ray tracing to simulate bullet mechanics, which helps the user kill zombies and survive longer. The zombies will start following the user based on the user's proximity using our pathfinding algorithm. The goal of the game is to protect the crystal and stay alive as long as possible. 

### Key Algorithms and Approaches

### Code

You can access the code [here](https://github.com/InfantDerrick/csci5611/tree/master/projects/proj3). 

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

For our projects success, we had to use a multitude of things we learned in class.
- RigidBody Simulation
- Colission Detection
- Path Finding
- Ray Tracing
- Terrain Generation using noise

I think the limiting factor when scaling this project would be the way that we implemented our opponents in this game. Currently how it is set is there are a limited amount of opponents pregenerated that are just rendered and de-rendered based on the location of the player is on. If the map parameters were set to be bigger than they are right now, then we would need to create way more enemies which would increase the size of the projects unreasonabley. If we were to proceed with this and try to deploy it where the enemies are spwaned in dynamically, the performance would not dip regardless of the size of them map. We were able to get the generation of the blocks that are generated to be dynamic but with the opponent having so many coponents and changes and animation, it was hard for us to deploy an opponent that was competent and followed all the rules of the game.

## Media

[Main Final Project Demo]( https://youtu.be/msnqzk_xdiY).
