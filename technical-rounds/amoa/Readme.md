
**Shortest path in binary maze**

    import java.util.*;

    public class Untitled{

      private static boolean isValid(int[][]a, int i, int j) {
        if(i<0||i>=a.length || j<0||j>=a[0].length)
          return false;
        return true;
      }

      private static int bfs(int[][] a, int i, int j) {
        Queue<Key> q = new LinkedList<>();
        Map<Key, Integer> map = new HashMap<>();

        Key src = new Key(0,0);
        Key dest = new Key(i,j);

        if(src.equals(dest))
          return 0;

        q.add(src);
        map.put(src, 0);
        while(!q.isEmpty()) {
          Key c = q.remove();
          if(isValid(a, c.x+1, c.y) && !map.containsKey(new Key(c.x+1, c.y)) && a[c.x+1][c.y]==1 ) {
            Key n = new Key(c.x+1, c.y);
            if (n.equals(dest))
              return map.get(c)+1;
            q.add(n);
            map.put(n, map.get(c)+1);
          }
          if(isValid(a, c.x-1, c.y) && !map.containsKey(new Key(c.x-1, c.y)) && a[c.x-1][c.y]==1 ) {
            Key n = new Key(c.x-1, c.y);
            if (n.equals(dest))
              return map.get(c)+1;
            q.add(n);
            map.put(n, map.get(c)+1);
          }
          if(isValid(a, c.x, c.y+1) && !map.containsKey(new Key(c.x, c.y+1)) && a[c.x][c.y+1]==1 ) {
            Key n = new Key(c.x, c.y+1);
            if (n.equals(dest))
              return map.get(c)+1;
            q.add(n);
            map.put(n, map.get(c)+1);
          }
          if(isValid(a, c.x, c.y-1) && !map.containsKey(new Key(c.x, c.y-1)) && a[c.x][c.y-1]==1 ) {
            Key n = new Key(c.x, c.y-1);
            if (n.equals(dest))
              return map.get(c)+1;
            q.add(n);
            map.put(n, map.get(c)+1);
          }
        }


        for(Map.Entry<Key, Integer> e : map.entrySet()) {
          System.out.println(e.getKey().x + ", " +  e.getKey().y +": " + e.getValue());
        }

        return -1;
      }

      public static void main(String []args){
        int[][] a= {{1, 0, 1, 1, 1, 1, 0, 1, 1, 1 },
              {1, 0, 1, 0, 1, 1, 1, 0, 1, 1 },
              {1, 1, 1, 0, 1, 1, 0, 1, 0, 1 },
              {0, 0, 0, 0, 1, 0, 0, 0, 0, 1 },
              {1, 1, 1, 0, 1, 1, 1, 0, 1, 0 },
              {1, 0, 1, 1, 1, 1, 0, 1, 0, 0 },
              {1, 0, 0, 0, 0, 0, 0, 0, 0, 1 },
              {1, 0, 1, 1, 1, 1, 0, 1, 1, 1 },
              {1, 1, 0, 0, 0, 0, 1, 0, 0, 1 }};


        System.out.println(bfs(a, 3, 4));

      }

      public static class Key {
        int x;
        int y;
        public Key(int x, int y) {
          this.x = x;
          this.y = y;
        }

        @Override
        public boolean equals(Object o) {
          if(this == o) return true;
          if(!(o instanceof Key)) return false;
          Key key = (Key) o;
          return x==key.x && y==key.y;
        }

        @Override
        public int hashCode() {
          int res = x;
          res = 31*res+y;
          return res;
        }
      }
    }

**Longest Pallindromic substring**

