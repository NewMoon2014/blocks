blocks
======

UVA blocks 101 problems
import java.io.IOException;
import java.util.Scanner;
import java.util.Stack;
import java.util.StringTokenizer;

 class Blocks {
	Stack<Integer>[] blocks;
	int[] position;

	public static void main(String[] args)  throws IOException{
		Blocks p = new Blocks();
		p.go();
		p.print();

	}

	private void print() {
		for (int i = 0; i < blocks.length; i++) {
			System.out.println(blockToString(i));
		}
	}

	private String blockToString(int i) {
		String ans = "";

		while (blocks[i].isEmpty() == false) {
			ans = " " + blocks[i].pop() + ans;
		}

		ans = i + ":" + ans;

		return ans;
	}
	//to let the compiler now that my program is legal at execution time 
	 @SuppressWarnings("unchecked")

	private void go() throws NumberFormatException, IOException{
		Scanner xx = new Scanner(System.in);
		String s = xx.nextLine();
		int n = Integer.parseInt(s);
		blocks = new Stack[n];
		position = new int[n];
		// make stack for each element
		for (int i = 0; i < n; i++) {
			blocks[i] = new Stack<Integer>();
			blocks[i].push(i);
			position[i] = i;
		}
		Scanner z = new Scanner(System.in);
		String l = "";
		while (!(l = z.nextLine()).equals("quit")) {

			StringTokenizer tokens = new StringTokenizer(l);
			String action = tokens.nextToken();
			int a = Integer.parseInt(tokens.nextToken());
			String type = tokens.nextToken();
			int b = Integer.parseInt(tokens.nextToken());
			if (a == b || position[a] == position[b])
				continue;
			if (action.equals("move")) {

				if (type.equals("onto"))
					moveOnto(a, b);
				else if (type.equals("over"))
					moveOver(a, b);

			} else if (action.equals("pile")) {
				if (type.equals("onto"))
					pileOnto(a, b);

				else if (type.equals("over"))
					pileOver(a, b);
			}

		}
	}

	public void moveOnto(int a, int b) {

		clearAbove(b);
		moveOver(a, b);
	}

	public void moveOver(int a, int b) {
		clearAbove(a);
		blocks[position[b]].push(blocks[position[a]].pop());
		position[a] = position[b];
	}

	public void pileOnto(int a, int b) {
		clearAbove(b);
		pileOver(a, b);
	}

	public void pileOver(int a, int b) {

		Stack<Integer> aPile = new Stack<Integer>();
		while (blocks[position[a]].peek() != a)
			aPile.push(blocks[position[a]].pop());

		aPile.push(blocks[position[a]].pop());

		while (!aPile.isEmpty()) {
			int top = aPile.pop();
			blocks[position[b]].push(top);
			position[top] = position[b];
		}
	}

	public void clearAbove(int x) {
		while (blocks[position[x]].peek() != x)
			returnHome(blocks[position[x]].pop());
	}

	private void returnHome(Integer x) {
		while (!blocks[x].isEmpty()) {
			returnHome(blocks[x].pop());
		}
		blocks[x].push(x);
		position[x] = x;
	}
}
