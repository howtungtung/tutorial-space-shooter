# tutorial-space-shooter
<img src="res/introduction.png" width="100%">

Unity官方範例 https://learn.unity.com/project/space-shooter-tutorial
# 創建基礎遊戲物件
創建飛機

![](res/1.png)
![](res/2.png)
![](res/3.png)
![](res/4.png)

設定攝影機

![](res/5.png)
![](res/6.png)
![](res/7.png)

設定光源

關閉環境光

![](res/8.png)

設定主光源

![](res/9.png)

設定測光源

![](res/10.png)

設定背光源

![](res/11.png)
![](res/12.png)

創建背景

![](res/13.png)
![](res/14.png)
![](res/15.png)
![](res/16.png)
![](res/17.png)

創建PlayerController掛在Player上

![](res/18.png)
# PlayerController.cs
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
  
    // Start is called before the first frame update
    void Start()
    {
    }

    // Update is called once per frame
    void Update()
    {
+       float moveHorizontal = Input.GetAxis("Horizontal");
+       float moveVertical = Input.GetAxis("Vertical");
+       Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
+       Debug.Log(movement);
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
+   private Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
+       rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void Update()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
-       Debug.Log(movement);
+       rb.velocity = movement;
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
+   public float speed = 10;
    private Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
-   void Update()
+   void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
+       rb.velocity = movement * speed;
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
+   public class Boundary
+   {
+       public float xMin;
+       public float xMax;
+       public float zMin;
+       public float zMax;
+   }

+   public Boundary boundary;
    public float speed = 10;
    
    private Rigidbody rb;
    
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.velocity = movement * speed;
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
+   [System.Serializable]
    public class Boundary
    {
        public float xMin;
        public float xMax;
        public float zMin;
        public float zMax;
    }
   
    public Boundary boundary;
    public float speed = 10;
    
    private Rigidbody rb;
    
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.velocity = movement * speed;
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [System.Serializable]
    public class Boundary
    {
        public float xMin;
        public float xMax;
        public float zMin;
        public float zMax;
    }
    
    public Boundary boundary;
    public float speed = 10;
    
    private Rigidbody rb;
    
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.velocity = movement * speed;
        
+       rb.position = new Vector3
+       (
+           Mathf.Clamp(rb.position.x, boundary.xMin, boundary.xMax),
+           0,
+           Mathf.Clamp(rb.position.z, boundary.zMin, boundary.zMax)
+       );
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [System.Serializable]
    public class Boundary
    {
        public float xMin;
        public float xMax;
        public float zMin;
        public float zMax;
    }
    
    public Boundary boundary;
    public float speed = 10;
    public float tilt = 4;
    private Rigidbody rb;
    
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.velocity = movement * speed;
+       rb.rotation = Quaternion.Euler(0, 0, rb.velocity.x * -tilt);
        rb.position = new Vector3
        (
            Mathf.Clamp(rb.position.x, boundary.xMin, boundary.xMax),
            0,
            Mathf.Clamp(rb.position.z, boundary.zMin, boundary.zMax)
        );
    }
}
```
![](res/19.png)
![](res/20.png)

創建子彈

![](res/21.png)
![](res/22.png)
![](res/23.png)
![](res/24.png)
![](res/25.png)
![](res/26.png)
![](res/27.png)

# Mover.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class Mover : MonoBehaviour
{
    public float speed = 20;
    private Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
        rb.velocity = transform.forward * speed;
    }
}
```
將子彈製作成預製物件

![](res/28.png)

# PlayerController.cs
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [System.Serializable]
    public class Boundary
    {
        public float xMin;
        public float xMax;
        public float zMin;
        public float zMax;
    }

    public float speed = 10;
    public float tilt = 4;
    public Boundary boundary;
+   public GameObject shot;
+   public Transform shotSpawn;
+   public float fireRate = 0.25f;

+   private float nextFire;
    private Rigidbody rb;

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
    }

+   private void Update()
+   {
+       if(Input.GetButton("Fire1") && Time.time > nextFire)
+       {
+           nextFire = Time.time + fireRate;
+           Instantiate(shot, shotSpawn.position, shotSpawn.rotation);
+       }    
+   }

    // Update is called once per frame
    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");
        float moveVertical = Input.GetAxis("Vertical");
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);
        rb.velocity = movement * speed;
        rb.rotation = Quaternion.Euler(0, 0, rb.velocity.x * -tilt);

        rb.position = new Vector3
        (
            Mathf.Clamp(rb.position.x, boundary.xMin, boundary.xMax),
            0,
            Mathf.Clamp(rb.position.z, boundary.zMin, boundary.zMax)
        );
    }
}
```
![](res/29.png)
![](res/30.png)

讓超出畫面的子彈消失

![](res/31.png)
![](res/32.png)
![](res/33.png)

# DestroyByBoundary.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByBoundary : MonoBehaviour
{
    private void OnTriggerExit(Collider other)
    {
        Destroy(other.gameObject);
    }
}
```
# 製作隕石
![](res/34.png)
![](res/35.png)

# RandomRotator.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class RandomRotator : MonoBehaviour
{
    public float tumble = 5;
    private Rigidbody rb;
    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();
        rb.angularVelocity = Random.insideUnitSphere * tumble;
    }
}
```
![](res/36.png)
![](res/37.png)

# DestroyByContact.cs
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByContact : MonoBehaviour
{
+   private void OnTriggerEnter(Collider other)
+   {
+       Destroy(other.gameObject);
+       Destroy(gameObject);
+   }
}
```

設定tag

![](res/38.png)
![](res/39.png)
![](res/40.png)

```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByContact : MonoBehaviour
{
    private void OnTriggerEnter(Collider other)
    {
+       if (other.CompareTag("Boundary"))
+       {
+           return;
+       }
        Destroy(other.gameObject);
        Destroy(gameObject);
    }
}
```
![](res/42.png)

新增爆炸特效

```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByContact : MonoBehaviour
{
+   public GameObject explosion;

    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Boundary"))
        {
            return;
        }
+       Instantiate(explosion, transform.position, transform.rotation);
        Destroy(other.gameObject);
        Destroy(gameObject);
    }
}
```
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByContact : MonoBehaviour
{
    public GameObject explosion;
+   public GameObject playerExplosion;

    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Boundary"))
        {
            return;
        }
        Instantiate(explosion, transform.position, transform.rotation);
+       if (other.CompareTag("Player"))
+       {
+           Instantiate(playerExplosion, transform.position, transform.rotation);
+       }
        Destroy(other.gameObject);
        Destroy(gameObject);
    }
}
```
![](res/43.png)
![](res/44.png)

掛上Mover.cs讓隕石會自己移動

![](res/45.png)

將隕石製作成預製物件

![](res/46.png)

# 完成遊戲流程
![](res/47.png)
# GameController.cs
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameController : MonoBehaviour
{
+   public GameObject hazard;
+   public Vector3 spawnValues;

    // Start is called before the first frame update
    void Start()
    {
+       SpawnWaves();
    }

-   // Update is called once per frame
-   void Update()
-   {
-       
-   }

+   private void SpawnWaves()
+   {
+       Vector3 spawnPosition = new Vector3(Random.Range(-spawnValues.x, spawnValues.x), spawnValues.y, spawnValues.z);
+       Quaternion spawnRotation = Quaternion.identity;
+       Instantiate(hazard, spawnPosition, spawnRotation);
+   }
}

```
![](res/48.png)

```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class GameController : MonoBehaviour
{
    public GameObject hazard;
    public Vector3 spawnValues;
+   public int hazardCount;
+   public float spawnWait;
+   public float startWait;
+   public float waveWait;

    // Start is called before the first frame update
    void Start()
    {
-       SpawnWaves();
+       StartCoroutine(SpawnWaves());
    }

-   private void SpawnWaves()
+   private IEnumerator SpawnWaves()
    {
+       yield return new WaitForSeconds(startWait);
+       while(true)
+       {
+           for (int i = 0; i < hazardCount; i++)
+           {
                Vector3 spawnPosition = new Vector3(Random.Range(-spawnValues.x, spawnValues.x), spawnValues.y, spawnValues.z);
                Quaternion spawnRotation = Quaternion.identity;
                Instantiate(hazard, spawnPosition, spawnRotation);
+               yield return new WaitForSeconds(spawnWait);
+           }
+           yield return new WaitForSeconds(waveWait);
+       }
    }
}
```
![](res/49.png)

將爆炸特效添加自我消滅

![](res/50.png)

# DestroyByTime.cs
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByTime : MonoBehaviour
{
+   public float lifetime;
    // Start is called before the first frame update
    void Start()
    {
+       Destroy(gameObject, lifetime);
    }

-   // Update is called once per frame
-   void Update()
-   {
-       
-   }
}
```

添加音效

![](res/51.png)
![](res/52.png)
![](res/53.png)

新增UI

![](res/54.png)

# GameController.cs
```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
+using UnityEngine.UI;
+using TMPro;

public class GameController : MonoBehaviour
{
    public GameObject hazard;
    public Vector3 spawnValues;
    public int hazardCount;
    public float spawnWait;
    public float startWait;
    public float waveWait;
+   public TextMeshProUGUI scoreText;
+   public int score;

    // Start is called before the first frame update
    void Start()
    {
+       score = 0;
+       UpdateScore();
        StartCoroutine(SpawnWaves());
    }

    private IEnumerator SpawnWaves()
    {
        yield return new WaitForSeconds(startWait);
        while(true)
        {
            for (int i = 0; i < hazardCount; i++)
            {
                Vector3 spawnPosition = new Vector3(Random.Range(-spawnValues.x, spawnValues.x), spawnValues.y, spawnValues.z);
                Quaternion spawnRotation = Quaternion.identity;
                Instantiate(hazard, spawnPosition, spawnRotation);
                yield return new WaitForSeconds(spawnWait);
            }
            yield return new WaitForSeconds(waveWait);
        }
    }

+   public void AddScore(int addScore)
+   {
+       score += addScore;
+       UpdateScore();
+   }

+   private void UpdateScore()
+   {
+       scoreText.text = "Score: " + score;    
+   }
}
```
![](res/55.png)

```diff
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByContact : MonoBehaviour
{
    public GameObject explosion;
    public GameObject playerExplosion;
+   public int scoreValues;
+   private GameController gameController;

+   private void Start()
+   {
+       GameObject gameControllerObject = GameObject.FindWithTag("GameController");
+       gameController = gameControllerObject.GetComponent<GameController>();
+   }

    private void OnTriggerEnter(Collider other)
    {
        if (other.CompareTag("Boundary"))
        {
            return;
        }
        Instantiate(explosion, transform.position, transform.rotation);
        if (other.CompareTag("Player"))
        {
            Instantiate(playerExplosion, transform.position, transform.rotation);
        }
+       gameController.AddScore(scoreValues);
        Destroy(other.gameObject);
        Destroy(gameObject);
    }
}
```
![](res/56.png)
![](res/57.png)
![](res/58.png)
![](res/59.png)
![](res/60.png)
![](res/61.png)

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using UnityEngine.SceneManagement;

public class GameController : MonoBehaviour
{
    public GameObject hazard;
    public Vector3 spawnValues;
    public int hazardCount;
    public float spawnWait;
    public float startWait;
    public float waveWait;
    public TextMeshProUGUI scoreText;
    public TextMeshProUGUI gameOverText;
    public TextMeshProUGUI restartText;
    public int score;
    private bool gameOver;
    private bool restart;

    // Start is called before the first frame update
    void Start()
    {
        score = 0;
        gameOver = false;
        restart = false;
        gameOverText.gameObject.SetActive(false);
        restartText.gameObject.SetActive(false);
        UpdateScore();
        StartCoroutine(SpawnWaves());
    }

    private void Update()
    {
        if(restart)
        {
            if (Input.GetKeyDown(KeyCode.R))
            {
                SceneManager.LoadScene(0);
            }
        }
    }

    private IEnumerator SpawnWaves()
    {
        yield return new WaitForSeconds(startWait);
        while(true)
        {
            for (int i = 0; i < hazardCount; i++)
            {
                Vector3 spawnPosition = new Vector3(Random.Range(-spawnValues.x, spawnValues.x), spawnValues.y, spawnValues.z);
                Quaternion spawnRotation = Quaternion.identity;
                Instantiate(hazard, spawnPosition, spawnRotation);
                yield return new WaitForSeconds(spawnWait);
            }
            yield return new WaitForSeconds(waveWait);
            if (gameOver)
            {
                restart = true;
                restartText.gameObject.SetActive(true);
                break;
            }
        }
    }

    public void AddScore(int addScore)
    {
        score += addScore;
        UpdateScore();
    }

    private void UpdateScore()
    {
        scoreText.text = "Score: " + score;    
    }

    public void GameOver()
    {
        gameOverText.gameObject.SetActive(true);
        gameOver = true;
    }
}
