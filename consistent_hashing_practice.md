# Consistent Hashing - Practice

In previous chapter we have got familiarize with concept that stands behind the Consistent Hashing algorithm. We know what kind of problem it solve and why this is a good fit for our distributed database. 

At this level of application I've decided to choose the first version of algorithm. We know it has disadvantages but its simplicity is good for starting the project. If you have the feeling to make it more reliable then feel free to create pull request (nevertheless, very good homework!).

To start the journey just download the project with [0.1 tag](wrong link).

We start with implementation of Ring.

```
case class Ring(underlying: SortedMap[Hash, Node] = TreeMap.empty) extends AnyVal {
  def size = underlying.size
  def nodesKey = underlying.values.map(_.key)
}

object Ring {

  def addNode(ring: Ring, nodeHash: Hash, node: Node): Ring = Ring(ring.underlying + ((nodeHash, node)))

  def getNode(ring: Ring, hash: Hash): Option[Node] = {
    if(ring.underlying.isEmpty)
      None
    else {
      val tailMap = ring.underlying.from(hash)
      val nodeHash = if(tailMap.isEmpty)
        ring.underlying.firstKey
      else
        tailMap.firstKey

      Some(ring.underlying(nodeHash))
    }
  }

}```

