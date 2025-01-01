# Design-3

## Problem 1: Flatten Nested List Iterator (https://leetcode.com/problems/flatten-nested-list-iterator/)
## Time and Space Complexity:O(n)
## Here we have  methods like next and hasNext the next is used to find the next integer while hasNext continues flattening until we get an Integer to return we always push the list in the stack in a reversed order so that we can pop it in proper order
/**
 * // This is the interface that allows for creating nested lists.
 * // You should not implement it, or speculate about its implementation
 * public interface NestedInteger {
 *
 *     // @return true if this NestedInteger holds a single integer, rather than a nested list.
 *     public boolean isInteger();
 *
 *     // @return the single integer that this NestedInteger holds, if it holds a single integer
 *     // Return null if this NestedInteger holds a nested list
 *     public Integer getInteger();
 *
 *     // @return the nested list that this NestedInteger holds, if it holds a nested list
 *     // Return empty list if this NestedInteger holds a single integer
 *     public List<NestedInteger> getList();
 * }
 */
public class NestedIterator implements Iterator<Integer> {
    private Stack<NestedInteger> stack;
    public NestedIterator(List<NestedInteger> nestedList) {
       stack=new Stack<>(); 

       for (int i = nestedList.size() - 1; i >= 0; i--) {
            stack.push(nestedList.get(i));
        }
    }

    @Override
    public Integer next() {
       return stack.pop().getInteger(); 
    }

    @Override
    public boolean hasNext() {

        while(!stack.isEmpty())
        {
            NestedInteger top=stack.peek();
            if(top.isInteger())
            {
                return true;
            }
            stack.pop();
        
            List<NestedInteger> nestedList = top.getList();
            for (int i = nestedList.size() - 1; i >= 0; i--) {
                stack.push(nestedList.get(i));}
        }
       return false; 
    }
    }    


/**
 * Your NestedIterator object will be instantiated and called as such:
 * NestedIterator i = new NestedIterator(nestedList);
 * while (i.hasNext()) v[f()] = i.next();
 */


## Problem 2: LRU Cache(https://leetcode.com/problems/lru-cache/)
## Time:O(1) and Space Complexity:O(capacity)
## Using linked list and dummy nodes we have successfully maintained adding and removal of nodes and adjusted wrt capacity accordingly.
class LRUCache {
    HashMap<Integer,Node> map;
    Node head,tail;
    int capacity;
    class Node{
    int key;
    int value;
    Node next,prev;

    public Node(int key,int value)
    {
        this.key=key;
        this.value=value;
    }
    }
    public LRUCache(int capacity) {
       this.capacity=capacity;
       this.map=new HashMap<>();
       this.head=new Node(-1,-1);
       this.tail=new Node(-1,-1);
       head.next=tail;
       tail.prev=head;
    }
    
    private void removeNode(Node curr)
    {
        curr.prev.next=curr.next;
        curr.next.prev=curr.prev; 
        curr.next=null;
        curr.prev=null;
    }

    private void addToHead(Node curr)
    {
        curr.next=head.next;
        curr.prev=head;
        head.next.prev = curr;
        head.next=curr;
    }
    
    public int get(int key) {
       if(!map.containsKey(key)) return -1;
       Node node=map.get(key);
       removeNode(node);
       addToHead(node);
       return node.value;
    }
    
    public void put(int key, int value) {
      if(map.containsKey(key)){
        Node node=map.get(key);
        node.value=value;
        removeNode(node);
        addToHead(node);

      } 
      else{
        if(map.size()==capacity){
        Node lruNode=tail.prev;
        removeNode(lruNode);
        map.remove(lruNode.key);
        }

        Node newNode=new Node(key,value);
        map.put(key,newNode);
        addToHead(newNode);
        
      }
    }
}

/**
 * Your LRUCache object will be instantiated and called as such:
 * LRUCache obj = new LRUCache(capacity);
 * int param_1 = obj.get(key);
 * obj.put(key,value);
 */
