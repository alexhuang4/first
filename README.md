# first
test
public class LinkedFrontBackCappedList<T extends Comparable<? super T>>
		implements FrontBackCappedListInterface<T>,
		Comparable<LinkedFrontBackCappedList<T>> {

	private Node head, tail;
	private final int MAXSIZE;
	private int numberOfEntries;

	public LinkedFrontBackCappedList(int max) {
		MAXSIZE = max;
		numberOfEntries = 0;
	}

	public boolean addFront(T newEntry) {
		if(numberOfEntries == MAXSIZE || newEntry == null) {
			return false;
		} else if(numberOfEntries == 0) {
			head = new Node(newEntry);
			tail = head;
		} else {
			Node temp = new Node(newEntry);
			temp.next = head;
			head = temp;
		}

		numberOfEntries++;
		return true;
	}

	public boolean addBack(T newEntry) {
		if(numberOfEntries == MAXSIZE || newEntry == null) {
			return false;
		} else if(numberOfEntries == 0) {
			head = new Node(newEntry);
			tail = head;
		} else {
			tail.next = new Node(newEntry);
			tail = tail.next;
		}
		numberOfEntries++;
		return true;
	}

	public T removeFront() {
		if(head == null) {
			return null;
		} else {
			if(numberOfEntries == 1) {
				tail = null;
			}
			T temp = head.getData();
			head = head.next;
			numberOfEntries--;
			return temp;
		}
	}

	public T removeBack() {
		if(tail == null) {
			return null;
		}
		T tempData;
		if(numberOfEntries == 1) {
			tempData = head.data;
			head = null;
			tail = null;
		} else {
			Node temp = head;
			while(temp.next.next != null) {
				temp = temp.next;
			}
			tempData = tail.getData();
			tail = temp;
			tail.next = null;
		}

		numberOfEntries--;
		return tempData;
	}

	public void clear() {
		head = null;
		tail = null;
		numberOfEntries = 0;
	}

	public T getEntry(int givenPosition) {
		if(givenPosition < 0 || givenPosition >= numberOfEntries) {
			return null;
		} else {
			Node temp = head;
			for(int i = 0; i < givenPosition; i++) {
				temp = temp.next;
			}
			return temp.getData();
		}
	}

	public int indexOf(T anEntry) {
		Node temp = head;
		for(int i = 0; i < numberOfEntries; i++) {
			if(temp.data.equals(anEntry)) {
				return i;
			}
			temp = temp.next;
		}
		return -1;
	}

	public int lastIndexOf(T anEntry) {
		Node temp = head;
		int lastIndex = -1;
		for(int i = 0; i < numberOfEntries; i++) {
			if(temp.data.equals(anEntry)) {
				lastIndex = i;
			}
			temp = temp.next;
		}
		return lastIndex;
	}

	public boolean contains(T anEntry){
		return indexOf(anEntry) >= 0;
	}

	public int size() {
		return numberOfEntries;
	}

	public boolean isEmpty() {
		return numberOfEntries == 0;
	}

	public boolean isFull() {
		return numberOfEntries == MAXSIZE;
	}

	@Override
	public String toString() {
		StringBuilder str = new StringBuilder("[");
		Node temp = head;
		while(temp != null) {
			str.append(temp.data);
			str.append(temp.next == null ? "" : ", ");
			temp = temp.next;
		}
		str.append("]\tsize=")
				.append(numberOfEntries)
				.append("\tcapacity=")
				.append(MAXSIZE)
				.append(head == null ? "" : " head=" + head.getData())
				.append(tail == null ? "" : " tail=" + tail.getData());
		return str.toString();
	}

	@Override
	@SuppressWarnings("unchecked")
	public int compareTo(LinkedFrontBackCappedList<T> other) {
		int otherSize = other.size();
		int thisSize = numberOfEntries;
		int result = 0;
        T[] thisArray = (T[])new Comparable[thisSize+1];
        T[] otherArray = (T[])new Comparable[otherSize+1];

        for(int i = 0; i < thisSize+otherSize; i++) {
            thisArray[i] = this.removeFront();
            otherArray[i] = other.removeFront();
            if(thisArray[i] == null && otherArray[i] == null) {
				break;
            } else if(thisArray[i] != null && otherArray[i] == null) {
                result = 1;
				break;
            } else if(thisArray[i] == null && otherArray[i] != null) {
                result = -1;
				break;
            } else if(!(thisArray[i].equals(otherArray[i]))) {
                result = thisArray[i].compareTo(otherArray[i]);
                break;
            }
        }

		fillNodeWithElements(thisArray, this);
		fillNodeWithElements(otherArray, other);
        return result;

	}

	private void fillNodeWithElements(T[] array, LinkedFrontBackCappedList list) {
	    for(int i = array.length-1; i >= 0; i--) {
	        list.addFront(array[i]);
        }
    }

	public class Node {
		public T data;
		public Node next;

		private Node(T dataPortion) {
			data = dataPortion;
			next = null;
		}

		private Node(T dataPortion, Node nextNode) {
			data = dataPortion;
			next = nextNode;
		}

		private T getData() {
			return data;
		}

		private void setData(T newData) {
			data = newData;
		}

		private Node getNextNode() {
			return next;
		}

		private void setNextNode(Node nextNode) {
			next = nextNode;
		}
	}
}
