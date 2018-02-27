# BruceFYPUnity
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.IO.Ports;
using System.Threading;
using System.Text;

public class ImportData : MonoBehaviour {

    SerialPort sp = new SerialPort("COM5", 9600);
    private string place;
    private float Xplace;
	private float rotX;
	private float rotY;
	private float rotZ;
    private Thread dataThread;
	private string[] data;


    // Use this for initialization
    void Start () {     
        dataThread = new Thread(new ThreadStart(Read));
        dataThread.IsBackground = true;
        dataThread.Start();
    }


	
	// Update is called once per frame
	void Update () {

    
		//print(rotX + " " + rotY + " " + rotZ);
		//print(place);

		Vector3 rota = new Vector3(rotX/30, rotZ/30, rotY/30);
        gameObject.transform.Rotate(rota, Space.World);

    }



       void Read()
        {
		sp.Open();
		while (sp.IsOpen) {
			Thread.Sleep (0);
			sp.ReadTimeout = 10;
			place = sp.ReadLine();

			char splitChar = ';';
			data = place.Split(splitChar);
			rotX = float.Parse(data [0]);
			rotY = float.Parse(data [1]);
			rotZ = float.Parse(data [2]);
			}
        }

        void OnDestroy()
        {
            dataThread.Abort();
        }
}

