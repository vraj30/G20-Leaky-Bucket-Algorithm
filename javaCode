
import java.util.LinkedList;
import java.util.Queue;
import java.util.Random;

public class LeakyBucket {
    private final int bucketCapacity; // Maximum capacity of the bucket
    private final int leakRate; // Packets that leak out per time unit
    private Queue<Integer> bucket; // Queue representing the bucket
    private int currentSize; // Current size of the bucket
    private int totalPacketsLost; // Total packets lost due to overflow

    public LeakyBucket(int capacity, int leakRate) {
        this.bucketCapacity = capacity;
        this.leakRate = leakRate;
        this.bucket = new LinkedList<>();
        this.currentSize = 0;
        this.totalPacketsLost = 0;
    }

    // Method to add packets to the bucket
    public void addPacket(int packets) {
        System.out.println("Incoming packets: " + packets);

        if (currentSize + packets <= bucketCapacity) {
            // Add packets to the bucket
            for (int i = 0; i < packets; i++) {
                bucket.add(1); // Each packet is represented as '1'
            }
            currentSize += packets;
            System.out.println("Packets added to bucket. Current bucket size: " + currentSize);
        } else {
            // Discard packets if the bucket overflows
            int discarded = currentSize + packets - bucketCapacity;
            totalPacketsLost += discarded;
            int accepted = packets - discarded;
            for (int i = 0; i < accepted; i++) {
                bucket.add(1);
            }
            currentSize = bucketCapacity;
            System.out.println("Bucket overflow! Packets discarded this time: " + discarded);
            System.out.println("Current bucket size: " + currentSize);
        }
    }

    // Method to process packets based on leak rate
    public void processPackets() {
        int packetsLeaked = Math.min(leakRate, currentSize);

        // Remove packets from the bucket
        for (int i = 0; i < packetsLeaked; i++) {
            bucket.poll();
        }
        currentSize -= packetsLeaked;

        System.out.println("Processed " + packetsLeaked + " packets. Current bucket size: " + currentSize);
    }

    public void logTotalPacketsLost() {
        System.out.println("\nTotal packets lost due to overflow: " + totalPacketsLost);
    }

    public static void main(String[] args) throws InterruptedException {
        int bucketCapacity = 5; // Maximum capacity of the bucket
        int leakRate = 3; // Leak rate per time unit
        LeakyBucket leakyBucket = new LeakyBucket(bucketCapacity, leakRate);

        Random random = new Random();

        // Simulating 10 time units of traffic
        for (int time = 1; time <= 10; time++) {
            System.out.println("\nTime unit: " + time);

            // Simulate incoming packets (random between 1 and 5)
            int incomingPackets = random.nextInt(5) + 1;
            leakyBucket.addPacket(incomingPackets);

            // Process packets according to leak rate
            leakyBucket.processPackets();

            // Log the cumulative packet loss for this time unit
            System.out.println("Cumulative packet loss so far: " + leakyBucket.totalPacketsLost);

            // Pause for 1 second to simulate real-time flow
            Thread.sleep(1000);
        }
        // Log total packets lost
        leakyBucket.logTotalPacketsLost();
    }
}
