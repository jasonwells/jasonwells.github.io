---
layout: post
title:  "Hibernate OneToMany Annotations"
date:   2009-07-15 20:34:00
categories: code java hibernate
---
As it was hard for me to find a clear example of a OneToMany join using Hibernate annotations in Java the other day, I decided to write this up.

This is an example of a unidirectional OneToMany join in Java using Hibernate. The JoinColumn name is a foreign key in the Message table. This will allow access to the Set of the messages that have been sent to the particular user. It automatically joins on the primary key of the User table.

{% highlight java %}
@Entity
@Table(name = "user")
public class User {
    @Id
    @GeneratedValue
    private Integer user_id;

    private String first_name;
    private String last_name;
    private String user_name;

    @OneToMany(cascade = CascadeType.ALL)
    @JoinColumn(name="to_user")
    private Set<Message> messagesTo;
}
{% endhighlight %}

This is the Message class that corresponds with the User class. The join is on the to_user column which is shown using a column annotation to specify a different name in the database. The join however will give a set of complete Message objects and not just the column joined.

{% highlight java %}
@Entity
@Table(name = "message")
public class Message {
    @Id
    @GeneratedValue
    private Integer message_id;

    @Column(name = "from_user")
    private Integer from;

    @Column(name = "to_user")
    private Integer to;

    @Column(name = "message_text")
    private String message;

    @Column(name = "date_sent")
    private Date date;
}
{% endhighlight %}

With this join you can name use a generic getMessagesTo() function that would allow you to easily retrieve the Set of all the messages sent to a particular user. The process can be easily repeated to retrieve the messages sent from a user as well.

It is important to note that this join is a unidirectional relationship. It does not provide for retrieving a User from an instance of Message.

