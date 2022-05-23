# Lab №3
<h3 align="center">Hello guys, i from NETI from Novosibirsk. And i love u all)
  
[![Typing SVG](https://readme-typing-svg.herokuapp.com?color=%2336BCF7&lines=Computer+science+student)](https://git.io/typing-svg)
## List
 - Первый пункт
   - Второй пункт
     - Третий пункт
  
 - And fourth PUNKT
 ## Рубрика крутый цитат ##
 > Самые крутой анекдот
  
  >> Что сказал слепой, войдя в бар?
  >>> "Всем привет, кого не видел".
  >>>> **Закадровый смех
  
### Книга про деда ###
  #### Глава 1 ####
  Дед родился
  #### Глава 2 ####
  Деда потеряли
  #### Глава 3 ####
  Деда нашли
  
  # А сейчас я покажу вам самый сложный код на C#
  
  ```C#
  void makePoint(HDC hDC, int x, int y, int size, int width, int height) 
  {
    POINT p1, p2;
    p1.x = START_CORDINATE + size * x;
    p1.y = START_CORDINATE + (height - 1) * size - size * y;
    p2.x = p1.x + size;
    p2.y = p1.y + size;
    L[x][y] = 1;
    Ellipse(hDC, p1.x, p1.y, p2.x, p2.y);
    Sleep(30);
 }
  ```
  ------------
  А сейчас самая жесткая функция
  ------------
  ```C#
  void SelectBrush(HDC hDC, double m) 
  {
    HPEN Brez1 = CreatePen(PS_SOLID, 2, RGB(255, 255, 255)); // Самая интенсивная
    HPEN Brez2 = CreatePen(PS_SOLID, 2, RGB(210, 210, 210));
    HPEN Brez3 = CreatePen(PS_SOLID, 2, RGB(180, 180, 180));
    HPEN Brez4 = CreatePen(PS_SOLID, 2, RGB(150, 150, 150));
    HPEN Brez5 = CreatePen(PS_SOLID, 2, RGB(120, 120, 120));
    HPEN Brez6 = CreatePen(PS_SOLID, 2, RGB(120, 120, 120));
    HPEN Brez7 = CreatePen(PS_SOLID, 2, RGB(90, 90, 90));
    HPEN Brez8 = CreatePen(PS_SOLID, 2, RGB(50, 50, 50));

    if (int(m) == 1)
        SelectObject(hDC, Brez1);
    else
        if (int(m) == 2)
            SelectObject(hDC, Brez2);
        else
            if (int(m) == 3)
                SelectObject(hDC, Brez3);
            else
                if (int(m) == 4)
                    SelectObject(hDC, Brez4);
                else
                    if (int(m) == 5)
                        SelectObject(hDC, Brez5);
                    else
                        if (int(m) == 6)
                            SelectObject(hDC, Brez6);
                        else
                            if (int(m) == 7)
                                SelectObject(hDC, Brez7);
                            else
                                if (int(m) == 8)
                                    SelectObject(hDC, Brez8);

}
  ```
  
  
  ```C#
 using UnityEngine;
using System.Collections;

[RequireComponent(typeof(Rigidbody2D))]

public class Player2DControl : MonoBehaviour {

	public enum ProjectAxis {onlyX = 0, xAndY = 1};
	public ProjectAxis projectAxis = ProjectAxis.onlyX;
	public float speed = 150;
	public float addForce = 7;
	public bool lookAtCursor;
	public KeyCode leftButton = KeyCode.A;
	public KeyCode rightButton = KeyCode.D;
	public KeyCode upButton = KeyCode.W;
	public KeyCode downButton = KeyCode.S;
	public KeyCode addForceButton = KeyCode.Space;
	public bool isFacingRight = true;
	private Vector3 direction;
	private float vertical;
	private float horizontal;
	private Rigidbody2D body;
	private float rotationY;
	private bool jump;

	void Start () 
	{
		body = GetComponent<Rigidbody2D>();
		body.fixedAngle = true;

		if(projectAxis == ProjectAxis.xAndY) 
		{
			body.gravityScale = 0;
			body.drag = 10;
		}
	}

	void OnCollisionStay2D(Collision2D coll) 
	{
		if(coll.transform.tag == "Ground")
		{
			body.drag = 10;
			jump = true;
		}
	}
	
	void OnCollisionExit2D(Collision2D coll) 
	{
		if(coll.transform.tag == "Ground")
		{
			body.drag = 0;
			jump = false;
		}
	}
	
	void FixedUpdate()
	{
		body.AddForce(direction * body.mass * speed);

		if(Mathf.Abs(body.velocity.x) > speed/100f)
		{
			body.velocity = new Vector2(Mathf.Sign(body.velocity.x) * speed/100f, body.velocity.y);
		}

		if(projectAxis == ProjectAxis.xAndY)
		{
			if(Mathf.Abs(body.velocity.y) > speed/100f)
			{
				body.velocity = new Vector2(body.velocity.x, Mathf.Sign(body.velocity.y) * speed/100f);
			}
		}
		else
		{
			if(Input.GetKey(addForceButton) && jump)
			{
				body.velocity = new Vector2(0, addForce);
			}
		}
	}

	void Flip()
	{
		if(projectAxis == ProjectAxis.onlyX)
		{
			isFacingRight = !isFacingRight;
			Vector3 theScale = transform.localScale;
			theScale.x *= -1;
			transform.localScale = theScale;
		}
	}
	
	void Update () 
	{
		if(lookAtCursor)
		{
			Vector3 lookPos = Camera.main.ScreenToWorldPoint(new Vector3(Input.mousePosition.x, Input.mousePosition.y, Camera.main.transform.position.z));
			lookPos = lookPos - transform.position;
			float angle  = Mathf.Atan2(lookPos.y, lookPos.x) * Mathf.Rad2Deg;
			transform.rotation = Quaternion.AngleAxis(angle, Vector3.forward);
		}

		if(Input.GetKey(upButton)) vertical = 1;
		else if(Input.GetKey(downButton)) vertical = -1; else vertical = 0;

		if(Input.GetKey(leftButton)) horizontal = -1;
		else if(Input.GetKey(rightButton)) horizontal = 1; else horizontal = 0;

		if(projectAxis == ProjectAxis.onlyX) 
		{
			direction = new Vector2(horizontal, 0); 
		}
		else 
		{
			if(Input.GetKeyDown(addForceButton)) speed += addForce; else if(Input.GetKeyUp(addForceButton)) speed -= addForce;
			direction = new Vector2(horizontal, vertical);
		}

		if(horizontal > 0 && !isFacingRight) Flip(); else if(horizontal < 0 && isFacingRight) Flip();
	}
}
}
  ```
  
  
  ## Разделители, и как это работает
  -----------------
  
  Разедитель первый
  
  -----------------
  
  Ого, уже воторой _____разделитель_____
  
  -----------------
  
  **ТРЕТИЙ**
  
  ----------------
  
 ~~А вот четвертого не будет~~
  
  ## Фотка с котом - смешная
  
![picture alt](https://newperexod.com/wp-content/uploads/2021/03/kupatsya-04.jpg)
  
