import java.rmi.Naming;

public class ServerClass {
	public static void main(String[] args) throws Exception {
		PalindromeInterface obj = (PalindromeInterface) new Palindrome();
		Naming.rebind("rmi://localhost/abc", obj);
		System.out.println("Server is ready");
	}
}


//clientclass

import java.net.MalformedURLException;
import java.rmi.Naming;
import java.rmi.NotBoundException;
import java.rmi.RemoteException;

public class ClientClass {
	public static void main(String[] args) throws MalformedURLException, RemoteException, NotBoundException {
		PalindromeInterface obj;
		obj = (PalindromeInterface) Naming.lookup("rmi://localhost/abc");
		Boolean b = obj.palindrome("level");
		if(b) {
			System.out.println("Palindrome");
		}else {
			System.out.println("Not a palindrome");
		}
	}
}




//Palindrome

import java.rmi.RemoteException;
import java.rmi.server.UnicastRemoteObject;

public class Palindrome extends UnicastRemoteObject implements PalindromeInterface{
	protected Palindrome() throws RemoteException{
		super();
	}
	
	public boolean palindrome(String str) {
		String rev = "";
		
		for(int i=str.length()-1;i>=0;i--) {
			rev = rev+str.charAt(i);
		}
		
		if(str.equals(rev)) {
			return true;
		}
		return false;
	}
}


import java.rmi.Remote;
import java.rmi.RemoteException;

public interface PalindromeInterface extends Remote{
	public boolean palindrome(String str) throws RemoteException;
}
