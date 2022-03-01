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
        rb = GetComponent<Rigidbody>();//自動關聯身上的rigidbody組件
        rb.velocity = transform.forward * speed;//對rigidbody設定速度
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
# PlayerController.cs
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class PlayerController : MonoBehaviour
{
    [System.Serializable]//特性Attribute，標記這個類別可以被序列化成文件，讓他可以在Inspector設定
    public class Boundary//宣告一個名為Boundary的類別，其目的用來記錄邊界的範圍
    {
        public float xMin;
        public float xMax;
        public float zMin;
        public float zMax;
    }

    public float speed = 10;//飛船移動的速度
    public float tilt = 4;//飛船左右移動時機身的傾斜度
    public Boundary boundary;//宣告一個Boundary類型的變數
    public GameObject shot;//用來關聯子彈的Prefab
    public Transform shotSpawn;//用來關聯子彈發射的位置參考
    public float fireRate = 0.25f;//子彈發射頻率

    private float nextFire;//記錄下一次可發射的時間
    private Rigidbody rb;//關聯身上的rigidbody組件
    private AudioSource audioSource;//關聯身上的audioSource組件

    // Start is called before the first frame update
    void Start()
    {
        rb = GetComponent<Rigidbody>();//自動關聯身上的rigidbody組件
        audioSource = GetComponent<AudioSource>();//自動關聯身上的audioSource組件
    }

    private void Update()
    {
        if (Input.GetButton("Fire1") && Time.time > nextFire)//每一幀判斷玩家是否有輸入"Fire1"的熱鍵組，且現在的時間要大於下一次能發射的時間
        {
            nextFire = Time.time + fireRate;//把下一次能發射的時候改成現在時間加上發射頻率
            Instantiate(shot, shotSpawn.position, shotSpawn.rotation);//生成一個子彈到shotSpawn的座標和旋轉到場景上
            audioSource.Play();//播放音效
        }
    }

    // Update is called once per frame
    void FixedUpdate()
    {
        float moveHorizontal = Input.GetAxis("Horizontal");//讀取玩家水平熱鍵組輸入的值(-1~1)
        float moveVertical = Input.GetAxis("Vertical");//讀取玩家垂直熱鍵組輸入的值(-1~1)
        Vector3 movement = new Vector3(moveHorizontal, 0.0f, moveVertical);//將輸入值存放在一個三維結構中
        rb.velocity = movement * speed;//將rigidbody的速度給予當前輸入值方向乘上一speed變數控制整體速度大小
        rb.rotation = Quaternion.Euler(0, 0, rb.velocity.x * -tilt);//根據當前x軸速度去傾斜飛機z軸角度

        rb.position = new Vector3//現在飛機能移動的邊界範圍
        (
            Mathf.Clamp(rb.position.x, boundary.xMin, boundary.xMax),//將給予值限定在指定的範圍內
            0,
            Mathf.Clamp(rb.position.z, boundary.zMin, boundary.zMax)
        );
    }
}
```
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
    private void OnTriggerExit(Collider other)//當有任意碰撞器離開此觸發器會自動被呼叫此方法
    {
        Destroy(other.gameObject);//刪除離開的碰撞器的遊戲物件
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
        rb = GetComponent<Rigidbody>();//自動關聯身上的rigidbody組件
        rb.angularVelocity = Random.insideUnitSphere * tumble;//對rigidbody設定角速度，使其自轉
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
# DestroyByContact.cs
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
# DestroyByContact.cs
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
# GameController.cs
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
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByTime : MonoBehaviour
{
    public float lifetime;
    // Start is called before the first frame update
    void Start()
    {
        Destroy(gameObject, lifetime);//設定幾秒後自我刪除
    }
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
# DestroyByContact.cs
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
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

public class DestroyByContact : MonoBehaviour
{
    public GameObject explosion;//關聯爆炸特效預製物件
    public GameObject playerExplosion;//關聯飛機爆炸預製物件
    public int scoreValues;//爆炸時要增加的分數
    private GameController gameController;//關聯GameController組件

    private void Start()
    {
        GameObject gameControllerObject = GameObject.FindWithTag("GameController");//找尋整個場景中tag為GameController的遊戲物件
        gameController = gameControllerObject.GetComponent<GameController>();//從該遊戲物件中關聯掛在身上的GameController組件
    }

    private void OnTriggerEnter(Collider other)//當有任意碰撞器進入此觸發器會自動被呼叫此方法
    {
        if (other.CompareTag("Boundary"))//如果進入的tag為Boundary則忽略
        {
            return;
        }
        Instantiate(explosion, transform.position, transform.rotation);//產生爆炸特效到當前物件所在的座標上
        if (other.CompareTag("Player"))//如果進入的tag為Player
        {
            Instantiate(playerExplosion, transform.position, transform.rotation);//產生飛機爆炸特效到當前物件所在的座標上
            gameController.GameOver();//呼叫GameController的GameOver方法，通知遊戲結束了
        }
        gameController.AddScore(scoreValues);//呼叫GameController的AddScore方法，增加分數
        Destroy(other.gameObject);//刪除進入的碰撞器的遊戲物件
        Destroy(gameObject);//刪除自己
    }
}
```
![](res/56.png)
![](res/57.png)
![](res/58.png)
![](res/59.png)
![](res/60.png)
![](res/61.png)
# GameController
```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.UI;
using TMPro;
using UnityEngine.SceneManagement;//為了要使用讀取場景的功能

public class GameController : MonoBehaviour
{
    public GameObject hazard;//關聯隕石的預製物件
    public Vector3 spawnValues;//設定產生隕石的參考座標
    public int hazardCount;//設定每一播隕石產生數量
    public float spawnWait;//設定每一顆隕石產生間隔
    public float startWait;//設定遊戲執行後延遲多久開始產生隕石
    public float waveWait;//設定每一播隕石的間隔
    public TextMeshProUGUI scoreText;//關聯顯示分數的文字
    public TextMeshProUGUI gameOverText;//關聯顯示遊戲結束的文字
    public TextMeshProUGUI restartText;//關聯顯示重新開始的文字
    public int score;//紀錄贏分
    private bool gameOver;//標記當前遊戲是否結束
    private bool restart;//標記當前是否可以重啟遊戲

    // Start is called before the first frame update
    void Start()
    {
        score = 0;//初始化參數
        gameOver = false;//初始化參數
        restart = false;//初始化參數
        gameOverText.gameObject.SetActive(false);//關閉文字顯示
        restartText.gameObject.SetActive(false);//關閉文字顯示
        UpdateScore();//更新分數文字
        StartCoroutine(SpawnWaves());//啟用一個協同程序
    }

    private void Update()
    {
        if (restart)//如果當前已可重啟
        {
            if (Input.GetKeyDown(KeyCode.R))//判斷玩家是否輸入R鍵
            {
                SceneManager.LoadScene(0);//重新讀取第一個場景
            }
        }
    }

    private IEnumerator SpawnWaves()//協同程序
    {
        yield return new WaitForSeconds(startWait);//等待startWait的時間
        while (true)//死迴圈，永遠執行
        {
            for (int i = 0; i < hazardCount; i++)//for迴圈，執行hazardCount的次數
            {
                Vector3 spawnPosition = new Vector3(Random.Range(-spawnValues.x, spawnValues.x), spawnValues.y, spawnValues.z);//隨機產生x軸範圍內的座標
                Quaternion spawnRotation = Quaternion.identity;//使用預設值套用在產生物件的旋轉
                Instantiate(hazard, spawnPosition, spawnRotation);//生成隕石到場景上，依照spawnPosition的位置和spawnRotation的旋轉
                yield return new WaitForSeconds(spawnWait);//等待spawnWait時間，再繼續執行下一次for迴圈
            }
            yield return new WaitForSeconds(waveWait);//等待waveWait時間
            if (gameOver)//如果遊戲結束了
            {
                restart = true;//標記可以重新開始
                restartText.gameObject.SetActive(true);//開啟重新開始的文字
                break;//中斷while迴圈
            }
        }
    }

    public void AddScore(int addScore)//提供給其他組件呼叫，可傳入分數
    {
        score += addScore;//將當前的分數累加上去
        UpdateScore();//呼叫刷新分數UI文字
    }

    private void UpdateScore()
    {
        scoreText.text = "Score: " + score;//更新分數文字內容
    }

    public void GameOver()//當飛機爆炸會被呼叫，代表遊戲結束了
    {
        gameOverText.gameObject.SetActive(true);//開啟遊戲結束的文字
        gameOver = true;//標記遊戲已結束的狀態
    }
}
```
