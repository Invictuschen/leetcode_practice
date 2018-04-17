# leetcode_practice
这题的目的是将所有可以把图从头至尾走完的路径表示出来
所以一开始知道要用List来存所得结果的队列
List<DirectedGraphNode> result=new ArrayList<DirectedGraphNode>();
每个点应该用一个hashmap来存储，表示当前点以及当前点是否有邻接的下一点
之后的想法是用bfs算法，把图上的点分别加入队列中，判断当前点是否有下一点，若有下一点，则给当前点的value值+1.若无下一点则当前点value值默认为1
Map<DirectedGraphNode,Integer> map=new HashMap<>();
for(DirectedGraphNode node:graph)
{//此处1层循环把图中的每个点找出，但是map中要存储的是每个点的neighbors关系，所以还要再遍历一层neighbors对象
   for(DirectedGraphNode neighbor:node.neighbors)
            {
                if(!map.containsKey(neighbor))
                {
                    map.put(neighbor,1);
                }
                else
                {
                    map.put(neighbor,map.get(neighbor)+1);
                }
            }
        }  //至此，图中的点的属性用map表示完成
        
        
        Queue<DirectedGraphNode> q=new LinkedList<>();
        //这个queue是用来装入第一个入点，若要求入点，则需要对入点进行判断
        for(DirectedGraphNode node:graph)
        {
            if(!map.containsKey(node))
            {
                q.add(node);
                result.add(node);
            }
        }
        
        while(!q.isEmpty())
        {
            DirectedGraphNode head=q.poll();//此时head为每次被检测的点
            // for(DirectedGraphNode node:graph)
            // {   这段无需再次重新遍历graph， 只要知道这个被检测的点的邻接点即可
                for(DirectedGraphNode neighbor:head.neighbors)
                {
                    if(map.containsKey(neighbor))
                    {
                        map.put(neighbor,map.get(neighbor)-1);
                    }
                    if(map.get(neighbor)==0)
                    {
                        q.add(neighbor);
                        result.add(neighbor);
                    }
                }
            // }
        }
        return result;
        
}
