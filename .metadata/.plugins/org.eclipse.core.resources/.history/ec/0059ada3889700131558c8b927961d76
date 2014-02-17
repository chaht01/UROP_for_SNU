import java.io.DataInputStream;
import java.io.DataOutputStream;
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;

//http://cremazer.blogspot.kr/2013/09/java-tcp.html
public class TcpIpMultichatServer {
	OutputStream clients;

	public void start(){
		ServerSocket serverSocket = null;
		Socket socket =null;
		try{
			serverSocket = new ServerSocket(7777);
			System.out.println("서버가 시작되었습니다.");
			while(true){
				socket = serverSocket.accept();
				System.out.println("["+socket.getInetAddress()+":"+socket.getPort()+"]"+"에서 접속하였습니다.");
				ServerReceiver thread = new ServerReceiver(socket);
				thread.start();
			}
		}catch(Exception e){
			e.printStackTrace();
		}
	}
	void sendToAll(String msg){
		try{
			DataOutputStream out = new DataOutputStream(clients);
			out.writeUTF(msg);
		}catch(IOException e){
		}
	}
	public static void main(String args[]){
		new TcpIpMultichatServer().start();
	}
	class ServerReceiver extends Thread{
		Socket socket;
		DataInputStream in;
		DataOutputStream out;
		ServerReceiver(Socket socket){
			this.socket = socket;
			try{
				in = new DataInputStream(socket.getInputStream());
				out = new DataOutputStream(socket.getOutputStream());
			}catch(IOException e){}
		}
		public void run(){
			String name = "";
			try{
				name = in.readUTF();
				sendToAll("#"+name+"님이 들어오셨습니다.");
				clients = out;
				while(in!=null){
					sendToAll(in.readUTF());
				}
			}catch(IOException e){
				//ignore
			}finally{
				sendToAll("#"+name+"님이 나갔습니다.");
				clients=null;
				System.out.println("["+socket.getInetAddress()+":"+socket.getPort()+"]"+"에서 접속을 종료하였씁니다.");
			}
		}
	}
}