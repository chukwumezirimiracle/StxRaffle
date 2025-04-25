# StxRaffle - Lottery Pool Smart Contract

## Overview

The **Lottery Pool Smart Contract** allows users to participate in a lottery by purchasing tickets, withdrawing tickets, selecting winners, and claiming prizes. It provides a transparent, automated, and fair lottery system on the Stacks blockchain, which supports various features such as a configurable ticket price, lottery duration, and withdrawal periods.

This smart contract is designed to ensure fairness, with multiple winners being randomly selected from the pool of participants. It also includes an organizer fee that is deducted from the prize pool.

## Features

- **Start a New Lottery**: Contract owner can configure a new lottery with custom parameters like duration, ticket price, winner count, and organizer fee.
- **Purchase Lottery Tickets**: Users can buy tickets for the lottery, which contributes to the prize pool.
- **Withdraw Tickets**: Users can withdraw tickets during the specified withdrawal period, with a refund of the ticket price.
- **Select Winners**: Contract owner can select random winners from the pool of participants once the lottery ends.
- **Claim Prizes**: Winners can claim their prize after the lottery has ended and winners have been selected.
- **Fee Distribution**: The contract deducts a configurable fee for the organizer from the prize pool.

## Constants

- **CONTRACT_OWNER**: The principal of the address that deploys and owns the contract.
- **ERR_NOT_AUTHORIZED**: Error thrown when an unauthorized user tries to interact with owner-only functions.
- **ERR_LOTTERY_INACTIVE**: Error thrown when trying to interact with an inactive lottery.
- **ERR_INSUFFICIENT_BALANCE**: Error thrown if the user does not have enough funds to purchase a ticket.
- **ERR_INVALID_TICKET_PRICE**: Error thrown when the ticket price is not valid (zero or negative).
- **ERR_NO_WINNERS**: Error thrown if there are no winners in the lottery.
- **ERR_NO_TICKETS**: Error thrown if the user does not have any tickets.
- **ERR_WITHDRAWAL_PERIOD_ENDED**: Error thrown when trying to withdraw tickets after the withdrawal period has ended.
- **ERR_LOTTERY_NOT_ENDED**: Error thrown if an attempt is made to end the lottery before its conclusion.
- **ERR_WINNERS_ALREADY_SELECTED**: Error thrown if winners have already been selected.
- **ERR_INVALID_DURATION**: Error thrown if the lottery duration is not valid.
- **ERR_INVALID_WITHDRAWAL_PERIOD**: Error thrown if the withdrawal period is not valid.
- **ERR_INVALID_WINNER_ID**: Error thrown when an invalid winner ID is provided.

## Data Variables

- **is-lottery-active**: Boolean indicating if the lottery is active.
- **current-ticket-price**: The current price of a lottery ticket.
- **current-lottery-pot**: The total pot accumulated from ticket sales.
- **total-tickets-sold**: The total number of tickets sold.
- **number-of-winners**: The number of winners to be selected.
- **lottery-end-block-height**: The block height at which the lottery ends.
- **withdrawal-end-block-height**: The block height after which users cannot withdraw tickets.
- **organizer-fee-percentage**: The percentage of the pot that the organizer will receive as a fee.
- **prize-per-winner**: The prize amount each winner will receive.
- **winners-selected**: Boolean indicating if the winners have been selected.

## Maps

- **ticket-ownership**: A map linking ticket IDs to the owner’s address.
- **user-ticket-count**: A map that tracks the number of tickets a user has purchased.
- **winners**: A map that stores the winners’ addresses and their claim status.

## Functions

### Public Functions

1. **start-new-lottery**
   - **Parameters**: 
     - `duration-in-blocks`: The number of blocks the lottery will run for.
     - `withdrawal-period`: The number of blocks allowed for ticket withdrawals.
     - `ticket-price`: The price of one lottery ticket.
     - `winner-count`: The number of winners to be selected.
     - `fee-percentage`: The percentage of the total prize pool that goes to the organizer.
   - **Description**: Starts a new lottery with the specified parameters. Only the contract owner can start a lottery.

2. **purchase-lottery-ticket**
   - **Description**: Allows users to purchase a lottery ticket. The ticket price is deducted from the user’s balance, and the ticket is added to the lottery pool.

3. **withdraw-tickets**
   - **Parameters**: 
     - `ticket-count`: The number of tickets the user wants to withdraw.
   - **Description**: Allows users to withdraw tickets within the allowed withdrawal period. The refunded amount is transferred back to the user.

4. **end-current-lottery**
   - **Description**: Ends the lottery and calculates the prize pool, deducting the organizer's fee. The prize per winner is calculated.

5. **select-winners**
   - **Parameters**: 
     - `random-seed`: A seed value for random number generation.
   - **Description**: Selects the lottery winners randomly. Only the contract owner can call this function, and winners can only be selected after the lottery ends.

6. **claim-prize**
   - **Parameters**: 
     - `winner-id`: The ID of the winner claiming their prize.
   - **Description**: Allows winners to claim their prize after they are selected. It ensures that each winner can only claim their prize once.

### Read-Only Functions

1. **get-current-ticket-price**
   - **Description**: Returns the current ticket price.

2. **get-current-lottery-pot**
   - **Description**: Returns the total amount in the lottery pot.

3. **get-user-ticket-count**
   - **Parameters**: 
     - `user-address`: The address of the user.
   - **Description**: Returns the number of tickets owned by the specified user.

4. **get-total-tickets-sold**
   - **Description**: Returns the total number of tickets sold in the lottery.

5. **check-if-lottery-is-active**
   - **Description**: Returns whether the lottery is active or not.

6. **get-lottery-end-block-height**
   - **Description**: Returns the block height at which the lottery will end.

7. **get-withdrawal-end-block-height**
   - **Description**: Returns the block height after which withdrawals are no longer allowed.

8. **get-organizer-fee-percentage**
   - **Description**: Returns the percentage of the prize pool that the organizer receives as a fee.

9. **get-winner-info**
   - **Parameters**: 
     - `winner-id`: The ID of the winner.
   - **Description**: Returns the information of the winner, including their address and claim status.

10. **are-winners-selected**
    - **Description**: Returns whether the winners have already been selected.

## Error Handling

The contract has a number of checks that ensure only valid actions are performed:

- **Authorization checks**: Only the contract owner can start a new lottery, end the lottery, or select winners.
- **Ticket purchase checks**: Ensures the user has sufficient balance and that the lottery is active before allowing ticket purchases.
- **Withdrawal checks**: Users can only withdraw tickets if they still have tickets and if the withdrawal period is still open.
- **Winner claim checks**: A winner can only claim their prize once, and the winner's claim status is tracked.

## Example Use Case

1. **Starting a New Lottery**: The contract owner can start a new lottery with a ticket price of 1 STX, 5 winners, and a 5% fee.
2. **Purchasing Tickets**: Users can purchase tickets by sending STX to the contract, increasing their chances of winning.
3. **Withdrawing Tickets**: If the user changes their mind, they can withdraw their tickets (before the withdrawal period ends) and receive a refund.
4. **Selecting Winners**: After the lottery duration ends, the owner selects random winners using the `select-winners` function.
5. **Claiming Prizes**: Winners can claim their prize after they are selected, with the prize being transferred to their address.

## Deployment

To deploy this contract on the Stacks blockchain, follow these steps:

1. **Install Clarinet**: Ensure you have Clarinet installed on your machine.
2. **Compile the Contract**: Use the `clarinet compile` command to compile the contract.
3. **Deploy the Contract**: Use the `clarinet deploy` command to deploy the contract on the Stacks network.
