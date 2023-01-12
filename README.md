# BinaryPatricaTrie

A is a Patricia Trie over the binary alphabet. By restricting themselves
to this small but terrifically useful alphabet, Binary Patricia Tries combine all the positive
aspects of Patricia Tries while shedding the storage cost typically associated with tries that
deal with huge alphabets


    public class BinaryPatriciaTrie {

    private static class TrieNode {
        private TrieNode left, right;
        private String str;
        private boolean isKey;

        // Default constructor for your inner nodes.
        TrieNode() {
            this("", false);
        }

        // Non-default constructor.
        TrieNode(String str, boolean isKey) {
            left = right = null;
            this.str = str;
            this.isKey = isKey;
        }
    }

    private TrieNode root;

    public BinaryPatriciaTrie() {
        this.root = new TrieNode();
    }


### Searches for key and returns true if key was found
    
    public boolean search(String key) {
        if(key == null || isEmpty() || root == null || key.equals(""))
        	return false;
        
        if(key.charAt(0) == '0')
        	return search_helper(key,root.left);
        else
        	return search_helper(key,root.right);
        
    }
    

    
    public boolean search_helper(String key, TrieNode curr_node)
    {
    	
    	if(curr_node == null)
    		return false;

    	String curr_str = curr_node.str;
    	
    	int sim = similarity(key,curr_str);
    	    	
    	//The words are the same
    	
    	if(curr_str.equals(key) )
    	{
    		if(curr_node.isKey)
    			return true;
    		else
    			return false;
    	}
    	else {
    		//Words are not the same
 
    		if(curr_str.length() >= key.length())
    			return false;

    			
    			String remainder = key.substring(sim);
    			
    			if(remainder.charAt(0) == '0')
    			{
    				//Go Left
    				//First check if its null, if it is then add and return
    				//If not proceed left recursively
    				
    				if(curr_node.left == null)
    				{
    					return false;
    				}
    				else {
    					return search_helper(remainder, curr_node.left);
    				}
    				
    				
    			}
    			else {
    				
    				//Go Right
    				//First check if its null, if it is then add and return
    				//If not proceed left recursively
    				
    				if(curr_node.right == null)
    				{
    					return false;
    				}
    				else {
    					return search_helper(remainder, curr_node.right);
    				}		
    			}
    	}
    }

   
### Inserts key into the trie. Returns true if it was inserted

    public boolean insert(String key) {
    	if(key == null)
    		return false;
    	
    	if(search(key))
    		return false;
    	
    	if(key.charAt(0) == '0')
    	{
    		if(root.left == null) {
    			root.left = new TrieNode(key, true);
    			return true;
    			
    		}
    		else {
    			root.left = insert_helper(key,root.left);
    			return true;
    		}
    	}
    	else {
    		if(root.right == null)
    		{
    			root.right = new TrieNode(key,true);
    			return true;
    		}
    		else {
    			root.right = insert_helper(key,root.right);
    			return true;
    		}
    	}

    }

    public TrieNode insert_helper(String key, TrieNode curr_node)
    {
    	String curr_str = curr_node.str;
    	
    	int sim = similarity(key,curr_str);
    	String match = key.substring(0,sim);
    	
    	//The words are the same
    	
    	if(curr_str.equals(key))
    	{
    		curr_node.isKey = true;
    		return curr_node;
    	}
    	else {
    		//Continue Traverse Case, but allocate if nothing ahead
    		if(key.length() > curr_str.length() && curr_str.length() == sim)
    		{
    			
    			String remainder = key.substring(sim);
    			if(remainder.charAt(0) == '0')
    			{
    				//Go Left
    				//First check if its null, if it is then add and return
    				//If not proceed left recursively	
    				if(curr_node.left == null)
    				{
    					TrieNode new_node = new TrieNode(remainder, true);
    					new_node.isKey = true;
    					curr_node.left = new_node;
    					return curr_node;
    				}
    				else {
    					curr_node.left = insert_helper(remainder, curr_node.left);
    					return curr_node;
    				}
    			}
    			else {	
    				//Go Right
    				//First check if its null, if it is then add and return
    				//If not proceed left recursively
    				
    				if(curr_node.right == null)
    				{
    					TrieNode new_node = new TrieNode(remainder, true);
    					new_node.isKey = true;
    					curr_node.right = new_node;
    					return curr_node;
    				}
    				else {
    					curr_node.right = insert_helper(remainder, curr_node.right);
    					return curr_node;
    				}
    				
    			}
    			
    			
    		}
    		//New Parent Case
    		else if(key.length() < curr_str.length())
    		{
    			
    			String child = curr_str.substring(sim);
    			String parent = curr_str.substring(0,sim);
    			
    			TrieNode new_parent = new TrieNode(parent,true);
    			
    			//Either send child left or right
    			if(child.charAt(0) == '0') {
    				curr_node.str = child;
    				new_parent.left = curr_node;
    				curr_node = new_parent;
    				return curr_node;
    				
    			}
    			else {
    				curr_node.str = child;
    				new_parent.right = curr_node;
    				curr_node = new_parent;
    				return curr_node;
    				
    			}
    			
    			
    		}
    		else {
    			 			
    			//Split Case
    			TrieNode new_parent = new TrieNode(curr_str.substring(0,sim), false);
    			String child = curr_str.substring(sim);
    			
    			if(child.charAt(0) == '0') {
    				curr_node.str = child;
    				new_parent.left = curr_node;
    				
    				new_parent.right = new TrieNode(key.substring(sim),true);
    				
    				return new_parent;
    				
    			}
    			else {
    				curr_node.str = child;
    				new_parent.right = curr_node;
    				
    				new_parent.left = new TrieNode(key.substring(sim),true);
    				
    				return new_parent;
    				
    			}
    			
    		}

    	}

    }
    
    public int similarity(String a, String b)
    {
    	int s = 0;
    	while(a.length() > s && b.length() > s)
    	{
    		if(a.charAt(s) == b.charAt(s))
    			s++;
    		else
    			break;
    	}  	
    	return s;
    }


    public TrieNode merge(TrieNode curr)
    {
    	if(curr == null)
    		return curr;
    	
    	if(curr.left == null && curr.right == null)
    		return curr;
    	else {
    		if(curr.isKey == false)
    		{
    			//Check if only left
    			if(curr.left != null && curr.right == null)
    			{
    				//Merge with left
    				TrieNode temp = new TrieNode(curr.str+curr.left.str,curr.left.isKey);
    				temp.left = curr.left.left;
    				temp.right = curr.left.right;
    				curr = temp;
    			}
    			
    			//Check if only right
    			if(curr.left == null && curr.right != null)
    			{
    				//Merge with right
    				TrieNode temp = new TrieNode(curr.str+curr.right.str,curr.right.isKey);
    				temp.left = curr.right.left;
    				temp.right = curr.right.right;
    				curr = temp;
    			}
    			
    		}
			curr.left = merge(curr.left);
			curr.right = merge(curr.right);
			return curr;
    	}
    }

### Deletes key from trie. Returns true if key was deleted.

    public boolean delete(String key) {
        if(!search(key))
        	return false;
        else {
        	 if(key.charAt(0) == '0') {
        		 root.left = delete_helper(key,root.left);
        		 
        		 root.left = merge(root.left);
        	
             	return true;
        	 }
             else {
             	root.right = delete_helper(key,root.right);
             	
             	root.right = merge(root.right);
        
        	 return true;
             }
        	
        }
    }
    

    
    public TrieNode delete_helper(String key, TrieNode curr_node)
    {
    	
    	if(curr_node == null)
    		return curr_node;


    	String curr_str = curr_node.str;
    	
    	int sim = similarity(key,curr_str);
    	
    	
    	//The words are the same
    	
    	if(curr_str.equals(key) )
    	{
    		//Found it
    		//Case 1 : more than 1 child
    		if(curr_node.left != null && curr_node.right != null)
    		{
    			curr_node.isKey = false;
    
    		}
    		else if(curr_node.left != null && curr_node.right == null) {
    			//Only 1 child on left
    			TrieNode temp = new TrieNode(curr_node.str+curr_node.left.str,true);
    			
    			temp.left = curr_node.left.left;
    			temp.right = curr_node.left.right;
    			temp.isKey = curr_node.left.isKey;

    			curr_node = temp;
	
    		}
    		else if(curr_node.left == null && curr_node.right != null) {
    			//Only 1 child on left
    			
    			TrieNode temp = new TrieNode(curr_node.str+curr_node.right.str,true);
    			
    			temp.left = curr_node.right.left;
    			temp.right = curr_node.right.right;
    			temp.isKey = curr_node.right.isKey;

    			curr_node = temp;

    		}
    		else {
    			//No Children, just purge it
    			curr_node = null;
    			
    		}
    		
    		return curr_node;
    		
    	}
    	else {
    		//Words are not the same

    			String remainder = key.substring(sim);
    			
    			if(remainder.charAt(0) == '0')
    			{
    				//Go Left
    				//First check if its null, if it is then add and return
    				//If not proceed left recursively
   
    				curr_node.left = delete_helper(remainder, curr_node.left);
    				return curr_node;
    			}
    			else {
    				
    				//Go Right
    				//First check if its null, if it is then add and return
    				//If not proceed left recursively

    				curr_node.right = delete_helper(remainder, curr_node.right);
    				return curr_node;
    				
    				}

    	}    	

    }
### Checkes if the trie is empty

    public boolean isEmpty() {
        if(root.left == null && root.right == null)
        	return true;
        else
        	return false;
    }

### Returns number of keys in Trie

    public int getSize() {
        if(isEmpty())
        	return 0;
        else 
        {
        	int a = size_helper(root.right);
        	int b = size_helper(root.left);
        	return a+b;
        }
        	
    }
    
    public int size_helper(TrieNode n) {
    	if(n == null)
    		return 0;
    	else
    	{
    		int a = size_helper(n.left) + size_helper(n.right);
    		
    		if(n.isKey)
    			a += 1;
    		return a;
    	}
    }

### Performs an inorder (symmetric) traversal of the Binary Patricia Trie

    public Iterator<String> inorderTraversal() {
        return new TrieIterator();
    }
    
    public class TrieIterator implements Iterator<String>{
    	private ArrayList<String> al;
    	private int index;
    	
    	public TrieIterator() {
    		al = new ArrayList<String>();
    		index = 0;
    		convert(root,"");
    		
    	}
    
		@Override
		public boolean hasNext() {
			if(index < al.size())
				return true;
			return false;
		}
		@Override
		public String next() {
			if(hasNext()) {
				
				return al.get(index++);
			}
			else {
				throw new NoSuchElementException();
			}
		}
		
		public void convert(TrieNode curr, String con) {
			//Left, Visit, Right

			if(curr == null)
				return;
			
			convert(curr.left,con+curr.str);
			if(curr.isKey)
				al.add(con+curr.str);
			convert(curr.right,con+curr.str);
					
		  }
    }
    
### Finds the longest string stored in the BPT

    public String getLongest() {
        
    	return getLongestHelper(root);
  	
    }
    
    public String getLongestHelper(TrieNode node) {
    	if(node == null)
    		return "";
    	else {
    		if(node.left == null && node.right == null)
    			return node.str;
    		else {
    			String l = getLongestHelper(node.left);
    			String r = getLongestHelper(node.right);
    			
    			if(l.length() > r.length())
    				return node.str+l;
    			else
    				return node.str+r;
    		}
    	}
    	
    }

### Ensures there isnt any splitter nodes with a single child and all nodes either denote stored strings or split into two subtrees

    public boolean isJunkFree(){
        return isEmpty() || (isJunkFree(root.left) && isJunkFree(root.right));
    }

    private boolean isJunkFree(TrieNode n){
        if(n == null){   // Null subtrees trivially junk-free
            return true;
        }
        if(!n.isKey){   // Non-key nodes need to be strict splitter nodes
            return ( (n.left != null) && (n.right != null) && isJunkFree(n.left) && isJunkFree(n.right) );
        } else {
            return ( isJunkFree(n.left) && isJunkFree(n.right) ); // But key-containing nodes need not.
        }
    }
  }
