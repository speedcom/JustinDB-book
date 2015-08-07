# Consistent Hashing - Practice

In previous chapter we have got familiarize with concept that stands behind the Consistent Hashing algorithm. We know what kind of problem it solve and why this is a good fit for our distributed database. 

At this level of application I've decided to choose the first version of algorithm. We know it has disadvantages but its simplicity is good for starting the project. If you have the feeling to make it more reliable then feel free to create pull request (nevertheless, very good homework!).

To start the journey just download the project with [0.1 tag](wrong link).

For first, take a look at dedicated tests for Ring.

```
class RingTest extends FlatSpec with Matchers {

  "Ring" should "be empty after initialization" in {
    new Ring().size shouldBe 0
  }

  it should "get None when Ring is empty" in {
    Ring.getNode(new Ring, hash = "100") shouldBe None
  }

  it should "allow to save Node with combined Hash" in {
    val node = Node(Key(value = "random-key"), underlyingActor = null)
    val nodeHash = "500"

    val ring = Ring.addNode(new Ring, nodeHash, node)

    ring.underlying should have size 1
  }

  it should "get proper Node based on different hashes" in {
    var ring = new Ring

    val node = Node(Key(value = "random-key-1"), underlyingActor = null)
    val nodeHash = "100"
    ring = Ring.addNode(ring, nodeHash, node)

    val node2 = Node(Key(value = "random-key-2"), underlyingActor = null)
    val nodeHash2 = "500"
    ring = Ring.addNode(ring, nodeHash2, node2)

    Ring.getNode(ring, hash = "-1").get shouldBe node
    Ring.getNode(ring, hash = "0").get shouldBe node
    Ring.getNode(ring, hash = "99").get shouldBe node
    Ring.getNode(ring, hash = "100").get shouldBe node
    Ring.getNode(ring, hash = "101").get shouldBe node2
    Ring.getNode(ring, hash = "300").get shouldBe node2
    Ring.getNode(ring, hash = "499").get shouldBe node2
    Ring.getNode(ring, hash = "500").get shouldBe node2
    Ring.getNode(ring, hash = "550").get shouldBe node
    Ring.getNode(ring, hash = "9000").get shouldBe node
  }

}```

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

