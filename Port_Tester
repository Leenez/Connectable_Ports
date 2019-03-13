// How to use threading to efficiently try which ports can be connected.

import java.net.Socket;
import java.util.ArrayList;

public class FindLocalOpenPorts {

	public static void main(String[] args) throws InterruptedException {
		
		ArrayList<int[]> testList = Storage.createPortList();
		ArrayList<Thread> threadList = new ArrayList<>();
		
		for(int[] s : testList) {
			threadList.add(new Thread(new FinderThread(s[0], s[1])));
		}
		
		int terminatedcount = 0;
		System.out.println("Starting port test ..." + "\n");
		while(terminatedcount < threadList.size()) {
			Thread.sleep(3000);
			int runnablecount = 0;
			for(Thread t : threadList) {
				if(t.getState().equals(Thread.State.RUNNABLE)) {
					runnablecount++;
				}
				if(t.getState().equals(Thread.State.TERMINATED)) {
					terminatedcount++;
				}
			}
			
			while(runnablecount < 400) {
				for(Thread t : threadList) {
					if(t.getState().equals(Thread.State.NEW)) {
						t.start();
						runnablecount++;
						break;
					}
				}
			}
		}
	}
}

class FinderThread implements Runnable{
	
	int start, stop;
		
	public FinderThread(int startPort, int stopPort) {
		start = startPort;
		stop = stopPort;
	}

	@Override
	public void run() {
		for(int i = start; i <= stop; i++) {
			try {
				Socket socketToTest = new Socket("localhost", i);
				Storage.printResult("TCP Port " + i + " Connects");
				socketToTest.close();
			} catch (Exception e) {
				//Storage.printResult("TCP Portti " + i + " KIINNI " + e);;
			} 
		}
	}
}

class Storage {
		
	static synchronized void printResult(String result) {
		System.out.println(result);
	}
	
	static ArrayList<int[]> createPortList() {
		ArrayList<int[]> spaceList = new ArrayList<>();
		int storelimit = 0;
		for(int i = 1; i < 65536; i++) {
			storelimit++;
			if(storelimit == 15) {
				int[] portSpace = new int[2];
				portSpace[0] = i - 14;
				portSpace[1] = i;
				spaceList.add(portSpace);
				storelimit = 0;
			}
		}
		return spaceList;
	}
}
