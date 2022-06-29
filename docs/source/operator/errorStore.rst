.. _managingProcessedMessagesError:

Error Stores
============

Purpose
-------

In the previous section :ref:`manageProcessedMessagesStore`, the fire-and-forget integration pattern was introduced. Your IT infrastructure applies this integration pattern when a service has to modify data. The fire-and-forget pattern means that no response is given to a request. The sender of the request trusts that it is processed correctly. The fire-and-forget pattern is also relevant when the sender does expect a response, because processing sometimes takes so long that the sender cannot wait for the response.

When the fire-and-forget pattern is applied, errors cannot be communicated to the request sender. Error handling is still required, however. One aspect of error handling is transactionality. This means that processing a request should either fully succeed or that no data should have changed when an error has occurred.

A classical example is when some person A pays some person B. The balance of A needs to be decreased while the balance of B needs to be increased. These two steps are typically grouped in a transaction. The transaction ensures the following: either both the decrement and the increment succeed, or no balance is changed. Without a transaction, it would be possible that A's balance is decremented without B's balance being incremented.

Implementing transactionality is the job of Frank developers and system administrators; you will not find features for that in the Frank!Console. When you are involved in acceptance testing you should certainly test that your backend processes are correctly grouped into transactions, but this is beyond the scope of this chapter.

When you apply the fire-and-forget pattern and when an error occurs, your transaction ensures that no data has been changed. The request sender still wants the request to be processed, however. This is the purpose of an error store. A Frank developer can add an error store tag (for example ``<JdbcErrorStorage>``) to a receiver. If the adapter behind the receiver fails to process a message, then the message is stored in the error store. In the Frank!Console, you can browse this error store to see for which messages processing failed. You can then investigate these errors and have your infrastructure fixed. When you know the issue has been resolved, you can use the Frank!Console to reissue some stored messages. Or you can delete messages when processing is no longer relevant.

In this section, you will learn how to use an error store in the Frank!Console.

Tutorial
--------

An error store has been added to the receiver of adapter 03. This adapter delegates its work to adapter 04. This allows you to choose whether adapter 03 shall succeed or fail. You can let adapter 03 fail by stopping adapter 04 and sending a message to adapter 01. Please start by checking that adapter 03 has not produced errors yet, as follows:

#. Expand adapter 03.
#. Under the heading "Receivers", you see receiver "03 MessageStoreReceiver". It has a "MessageStoreListener" as you saw before. At the end of this row, to the far right, you see the symbol shown below:

   .. image:: errorStoreSymbol.jpg

#. Check that the error store symbol has a number "0" next to it.

You have verified that no errors have been created yet. Please continue by introducing an error, as follows:

4. Stop adapter 04.
#. Using Test a Pipeline, write a message to adapter 01. Please make it different from all messages you sent so far.
#. Return to the Adapter Status page.
#. Note that adapter 03 has become red. The Frank!Console notifies you that something went wrong.
#. Expand adapter 03.
#. Check that the error store symbol has a number "1" now. This verifies that you produced an error.
#. Click the error store symbol. You should see the following page:

   .. image:: errorStoreMessages.jpg

#. You see a table of messages for which an error occurred. Each message has four buttons "View", "Resend", "Delete" and "Download". Column "Comment" shows an explanation of the error. There is also a column for the storage id (highlighted). Above the table, there is an expandable line "Display and Search Filters" (expand button highlighted).

You know the buttons "View" and "Download" already: they allow you to view or download the message. The explanation of the error is more interesting. It reads: "too many retries; Pipe [Send] msgId [5] caught exception: could not find JavaListener [04 JavaListener]". This allows you to verify that the error was caused by stopping adapter 04. Please do the following:

12. Press the "Back" button to the top right.
#. Expand adapter 04 and look under the heading "Receivers". You see the following: "JavaListener [04 JavaListener] (internal: 04 JavaListener)".
#. Compare this text with the error you saw. This is the missing listener.

The error text also said "too many retries". Please continue to see the consequence of this message:

15. Start adapter 04.
#. Return to adapter 03 and watch the error store symbol. Please note that adapter 03 remains red and that the number of errors remains 1.

Adapter 03 has been created to give up soon, allowing you to see the error store in action. Real adapters with an error store may be more patient. In your production environment, resending the error message may be retried automatically. After such a succesful retry, the error message is removed from the error store. Please continue as follows to resend the error message manually:

17. Click the error store symbol of adapter 3.
#. Press "Resend". Your screen should look like shown below. There is a green bar indicating success (highlighted) that also shows the id of the sent message. You see that the error message is no longer present (placeholder highlighted).

   .. image:: managingProcessedMessageSucessfullyResent.jpg

#. Press "Back".
#. Check that adapter 03 is green again. You have fixed the issue with it.

This finishes the tutorial on error stores. You have learned that an error store is needed to complete the fire-and-forget integration pattern. In this pattern, the sender does not get a response, so possible errors cannot be indicated that way. An error store allows for retrying failed messages. It also allows you as a site owner to resend a message manually, which you can do after fixing the cause of the error.