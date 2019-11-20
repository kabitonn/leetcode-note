# 355. Design Twitter(M)

[355. 设计推特](https://leetcode-cn.com/problems/design-twitter/)

## 题目描述(中等)

设计一个简化版的推特(Twitter)，可以让用户实现发送推文，关注/取消关注其他用户，能够看见关注人（包括自己）的最近十条推文。你的设计需要支持以下的几个功能：

1. `postTweet(userId, tweetId)`: 创建一条新的推文
2. `getNewsFeed(userId)`: 检索最近的十条推文。每个推文都必须是由此用户关注的人或者是用户自己发出的。推文必须按照时间顺序由最近的开始排序。
3. `follow(followerId, followeeId)`: 关注一个用户
4. `unfollow(followerId, followeeId)`: 取消关注一个用户

示例:
```
Twitter twitter = new Twitter();

// 用户1发送了一条新推文 (用户id = 1, 推文id = 5).
twitter.postTweet(1, 5);

// 用户1的获取推文应当返回一个列表，其中包含一个id为5的推文.
twitter.getNewsFeed(1);

// 用户1关注了用户2.
twitter.follow(1, 2);

// 用户2发送了一个新推文 (推文id = 6).
twitter.postTweet(2, 6);

// 用户1的获取推文应当返回一个列表，其中包含两个推文，id分别为 -> [6, 5].
// 推文id6应当在推文id5之前，因为它是在5之后发送的.
twitter.getNewsFeed(1);

// 用户1取消关注了用户2.
twitter.unfollow(1, 2);

// 用户1的获取推文应当返回一个列表，其中包含一个id为5的推文.
// 因为用户1已经不再关注用户2.
twitter.getNewsFeed(1);
```

## 思路

面向对象设计

## 解决方法

### 面向对象设计

类设计
- Tweet
  - tweetId
  - timestamp
- User
  - userId
  - tweets：所发布的tweet，时间逆序排列(最新的在最前面)
  - followers：关注自己的人
  - followings：关注的人
  - post()：发布tweet
  - following()：关注其他人
  - unfollowing()：取消关注其他人
  - followed()：被其他人关注
  - unfollowed()：被其他人取消关注
-Twitter
  - userMap：用户表
  - tweetNum：查看tweet数目
  - timestamp：时间戳

算法设计

获取k个有序链表的前num个节点，采用优先队列，每条链表中至多取num个节点加入队列，最后取出num个即为所求

```java
public class Twitter {

    public static int timestamp = 1;
    public static int tweetNum = 10;
    Map<Integer, User> userMap;

    /**
     * Initialize your data structure here.
     */
    public Twitter() {
        userMap = new HashMap<>();
    }

    /**
     * Compose a new tweet.
     */
    public void postTweet(int userId, int tweetId) {
        if (!userMap.containsKey(userId)) {
            userMap.put(userId, new User(userId));
        }
        User user = userMap.get(userId);
        user.post(tweetId);
    }

    /**
     * Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followings or by the user herself. Tweets must be ordered from most recent to least recent.
     */
    public List<Integer> getNewsFeed(int userId) {
        if (!userMap.containsKey(userId)) {
            return new ArrayList<>();
        }
        PriorityQueue<Tweet> minHeap = new PriorityQueue<>(tweetNum + 1, new Comparator<Tweet>() {
            @Override
            public int compare(Tweet o1, Tweet o2) {
                return o1.timestamp - o2.timestamp;
            }
        });
        Set<Integer> followings = userMap.get(userId).getFollowings();
        for (int id : followings) {
            List<Tweet> tweets = userMap.get(id).getTweets();
            for (int i = 0; i < tweets.size() && i < tweetNum; i++) {
                if (minHeap.size() < tweetNum) {
                    minHeap.add(tweets.get(i));
                } else {
                    minHeap.add(tweets.get(i));
                    minHeap.poll();
                }
            }
        }
        List<Integer> list = new LinkedList<>();
        while (!minHeap.isEmpty()) {
            list.add(0, minHeap.poll().getTweetId());
        }
        return list;
    }

    /**
     * Follower follows a followee. If the operation is invalid, it should be a no-op.
     */
    public void follow(int followerId, int followeeId) {
        if (!userMap.containsKey(followerId)) {
            userMap.put(followerId, new User(followerId));
        }
        User follower = userMap.get(followerId);
        follower.following(followeeId);
        if (!userMap.containsKey(followeeId)) {
            userMap.put(followeeId, new User(followeeId));
        }
    }

    /**
     * Follower unfollows a followee. If the operation is invalid, it should be a no-op.
     */
    public void unfollow(int followerId, int followeeId) {
        if (userMap.containsKey(followerId)) {
            User follower = userMap.get(followerId);
            follower.unfollowing(followeeId);
        }
    }
}

class User {
    int userId;
    List<Tweet> tweets;
    Set<Integer> followings;
    Set<Integer> followers;

    public User() {
        tweets = new LinkedList<>();
        followings = new HashSet<>();
        followers = new HashSet<>();
    }

    public User(int userId) {
        this.userId = userId;
        tweets = new LinkedList<>();
        followings = new HashSet<>();
        followers = new HashSet<>();
        followings.add(userId);
    }

    public void post(int tweetId) {
        Tweet tweet = new Tweet(tweetId, Twitter.timestamp++);
        tweets.add(0, tweet);
    }

    public void following(int followeeId) {
        followings.add(followeeId);
    }

    public void unfollowing(int followeeId) {
        if (followeeId != userId) {
            followings.remove(followeeId);
        }
    }

    public void followed(int followerId) {
        followers.add(followerId);
    }

    public void unfollowed(int followerId) {
        followings.remove(followerId);
    }

    public int getUserId() {
        return userId;
    }

    public List<Tweet> getTweets() {
        return tweets;
    }

    public Set<Integer> getFollowings() {
        return followings;
    }

    public Set<Integer> getFollowers() {
        return followers;
    }
}

class Tweet {
    int tweetId;
    int timestamp;

    public Tweet() {
    }

    public Tweet(int tweetId, int timestamp) {
        this.tweetId = tweetId;
        this.timestamp = timestamp;
    }

    public int getTweetId() {
        return tweetId;
    }

    public int getTimestamp() {
        return timestamp;
    }
}
```

### 简要设计

```java
public class Twitter_1 {

    public static int timestamp = 1;
    public static int tweetNum = 10;

    private Map<Integer, List<Tweet>> tweetMap;
    private Map<Integer, Set<Integer>> followingMap;

    /**
     * Initialize your data structure here.
     */
    public Twitter_1() {
        tweetMap = new HashMap<>();
        followingMap = new HashMap<>();
    }

    /**
     * Compose a new tweet.
     */
    public void postTweet(int userId, int tweetId) {
        if (!tweetMap.containsKey(userId)) {
            tweetMap.put(userId, new LinkedList<>());
            follow(userId, userId);
        }
        tweetMap.get(userId).add(0, new Tweet(tweetId, timestamp++));
    }

    /**
     * Retrieve the 10 most recent tweet ids in the user's news feed. Each item in the news feed must be posted by users who the user followed or by the user herself. Tweets must be ordered from most recent to least recent.
     */
    public List<Integer> getNewsFeed(int userId) {
        if (!tweetMap.containsKey(userId)) {
            return new ArrayList<>();
        }
        PriorityQueue<Tweet> minHeap = new PriorityQueue<>(tweetNum + 1, new Comparator<Tweet>() {
            @Override
            public int compare(Tweet o1, Tweet o2) {
                return o1.timestamp - o2.timestamp;
            }
        });
        Set<Integer> followings = followingMap.get(userId);
        for (int id : followings) {
            List<Tweet> tweets = tweetMap.get(id);
            for (int i = 0; i < tweets.size() && i < tweetNum; i++) {
                minHeap.add(tweets.get(i));
                if (minHeap.size() > tweetNum) {
                    minHeap.poll();
                }
            }
        }
        List<Integer> list = new LinkedList<>();
        while (!minHeap.isEmpty()) {
            list.add(0, minHeap.poll().getTweetId());
        }
        return list;
    }

    /**
     * Follower follows a followee. If the operation is invalid, it should be a no-op.
     */
    public void follow(int followerId, int followeeId) {
        if (!followingMap.containsKey(followerId)) {
            followingMap.put(followerId, new HashSet<>());
        }
        followingMap.get(followerId).add(followeeId);
        if (!tweetMap.containsKey(followerId)) {
            tweetMap.put(followerId, new LinkedList<>());
            follow(followerId, followerId);
        }
        if (!tweetMap.containsKey(followeeId)) {
            tweetMap.put(followeeId, new LinkedList<>());
            follow(followeeId, followeeId);
        }
    }

    /**
     * Follower unfollows a followee. If the operation is invalid, it should be a no-op.
     */
    public void unfollow(int followerId, int followeeId) {
        if (followerId != followeeId && followingMap.containsKey(followerId)) {
            followingMap.get(followerId).remove(followeeId);
        }
    }
}

```
