
## Paradigm:

1. Choose number from list and store it in chosen
2. If chosen contains a possible result
    - Add to list of lists
    - Else recurse
3. Undo changes and proceed

## Functions:

- **void permute(list, chosen, res)**: Backtracking algorithm that gets permutations of list and stores them in result

## Backtracking algorithm

      private void permute(List<Integer> list, List<Integer> chosen, List<List<Integer>> res) {
          if(list.isEmpty()) {
              res.add(new ArrayList<Integer>(chosen));
          } else {
              for(int i=0; i< list.size(); i++) {

                  // choose element from list and add to chosen
                  int x = list.get(i);
                  chosen.add(x);
                  list.remove(i);

                  // Recurse
                  permute(list, chosen, res);

                  // Undo changes
                  list.add(i, x);
                  chosen.remove(chosen.size()-1);
              }
          }
      }

- If list is empty, chosen contains a possible result. Add it to list of results
- Sequentially choose elements from list and add them to chosen
    - Permute the remaining elements
- Unchoose element from list by adding it back to the list and removing from chosen.

## Full code

Full code as follows:

  class Solution {

      private void permute(List<Integer> list, List<Integer> chosen, List<List<Integer>> res) {
          if(list.isEmpty()) {
              res.add(new ArrayList<Integer>(chosen));
          } else {
              for(int i=0; i< list.size(); i++) {

                  // choose element from list and add to chosen
                  int x = list.get(i);
                  chosen.add(x);
                  list.remove(i);

                  // Recurse
                  permute(list, chosen, res);

                  // Undo changes
                  list.add(i, x);
                  chosen.remove(chosen.size()-1);
              }
          }
      }

      public List<List<Integer>> permute(int[] nums) {

          List<List<Integer>> res = new ArrayList<>();
          List <Integer> chosen = new ArrayList<>();
          List <Integer> list = new ArrayList<>();

          for(int i: nums){
              list.add(i);
          }

          permute(list, chosen, res);
          return res;

      }
  }
