Note: This will be edited later

Echaustive search - gives TLE; can be made better using BFS

    class Solution {

        private boolean can_change(String w1, String w2) {
            int sum = 0;
            for(int i = 0; i<w1.length(); i++) {
                if(w1.charAt(i) != w2.charAt(i))
                    sum++;
            }
            if(sum == 1)
                return true;
            return false;
        }

        private void buildLadder(String current, String end, List<String> wordList, Set<String> chosen, int[] len) {
            if(current.equals(end)) {                           // BASE CASE
                if(chosen.size()+1 < len[0])
                    len[0] = chosen.size()+1;
            } else {                                            // INDUCTION STEP
                for (int i = 0; i<wordList.size(); i++) {  
                    String s = wordList.get(i);
                    if( can_change(current, s) ) {

                        // Add to ladder
                        wordList.remove(i);
                        chosen.add(s);

                        // Recurse
                        buildLadder(s, end, wordList, chosen, len);


                        // Backtrack
                        wordList.add(i, s);
                        chosen.remove(s);

                    }
                }

            } 
        }

        public int ladderLength(String beginWord, String endWord, List<String> wordList) {
            Set <String> chosen = new TreeSet<>();
            int len[] = new int[1];
            len[0] = Integer.MAX_VALUE;


            if(!wordList.contains(endWord))
                return 0;

            buildLadder(beginWord, endWord, wordList, chosen, len);
            if(len[0] == Integer.MAX_VALUE)
                return 0;
            return len[0];

        }
    }
