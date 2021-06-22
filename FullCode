
import simplegui
import random

# load card sprite - 936x384 - source: jfitz.com
CARD_SIZE = (72, 96)
CARD_CENTER = (36, 48)
card_images = simplegui.load_image("http://storage.googleapis.com/codeskulptor-assets/cards_jfitz.png")

CARD_BACK_SIZE = (72, 96)
CARD_BACK_CENTER = (36, 48)
card_back = simplegui.load_image("http://storage.googleapis.com/codeskulptor-assets/card_jfitz_back.png")    

# initialize some useful global variables
in_play = False
outcome = ""
score = 0
bet = 0
score1 = 0
# define globals for cards
SUITS = ('C', 'S', 'H', 'D')
RANKS = ('A', '2', '3', '4', '5', '6', '7', '8', '9', 'T', 'J', 'Q', 'K')
VALUES = {'A':1, '2':2, '3':3, '4':4, '5':5, '6':6, '7':7, '8':8, '9':9, 'T':10, 'J':10, 'Q':10, 'K':10}


# define card class
class Card:
    def __init__(self, suit, rank):
        if (suit in SUITS) and (rank in RANKS):
            self.suit = suit
            self.rank = rank
        else:
            self.suit = None
            self.rank = None
            print "Invalid card: ", suit, rank

    def __str__(self):
        return self.suit + self.rank

    def get_suit(self):
        return self.suit

    def get_rank(self):
        return self.rank

    def draw(self, canvas, pos):
        card_loc = (CARD_CENTER[0] + CARD_SIZE[0] * RANKS.index(self.rank), 
                    CARD_CENTER[1] + CARD_SIZE[1] * SUITS.index(self.suit))
        canvas.draw_image(card_images, card_loc, CARD_SIZE, [pos[0] + CARD_CENTER[0], pos[1] + CARD_CENTER[1]], CARD_SIZE)
        
# define hand class
class Hand:
    def __init__(self):
        self.h_card = []

    def __str__(self):
        xx = ""
        for i in self.h_card:
            xx = xx +" "+ str(i) 
        return "hand contains" + xx    
            
    def add_card(self, card):
        self.h_card.append(card)

    def get_value(self):
        value = 0
        for i in self.h_card:
            value += VALUES[i.get_rank()]
        if len(self.h_card) > 0:    
            for j in self.h_card:    
                if j.get_rank() != "A":
                    return value
                else :
                    if value + 10 <= 21:
                        return value + 10
                    else:
                        return value
        else :
            value = 0 
            return value
   
    def draw(self, canvas, pos):
        for i in self.h_card :
            card_loc = (CARD_CENTER[0] + CARD_SIZE[0] * RANKS.index(i.get_rank()), 
                    CARD_CENTER[1] + CARD_SIZE[1] * SUITS.index(i.get_suit()))
            canvas.draw_image(card_images, card_loc, CARD_SIZE, [pos[0] + CARD_CENTER[0] + 80*(self.h_card.index(i)), pos[1] + CARD_CENTER[1]], CARD_SIZE)        
        
# define deck class 
class Deck:
    def __init__(self):
        self.d_card = ""
        self.deck = []
        for i in SUITS:
            for j in RANKS :
                self.deck.append(Card(i,j))

    def shuffle(self):
        random.shuffle(self.deck)
        
    def deal_card(self):
        self.d_card = random.choice(self.deck)
        self.deck.remove(self.d_card)
        return self.d_card

    def __str__(self):
        xx = ""
        for i in self.deck :
            xx = xx + " " + str(i)
        return "deck contains " + xx

    
        
#define event handlers for buttons
def deal():
    global outcome, in_play, deck, pl_hand,de_hand,score,bet,score1
    if in_play:
        outcome = "You lost, deal during a round!"
        in_play = False
        score -= 1   
        score1 -= bet
    else:    
        in_play = True
        deck = Deck()
        deck.shuffle()
        pl_hand = Hand()
        pl_hand.add_card(deck.deal_card())
        pl_hand.add_card(deck.deal_card())   
        de_hand = Hand()    
        de_hand.add_card(deck.deal_card())
        de_hand.add_card(deck.deal_card())
        
def hit():
    global outcome, in_play, deck, pl_hand,de_hand ,score,score1,bet
    if in_play :
        if pl_hand.get_value() <= 21 :
            pl_hand.add_card(deck.deal_card())
            if pl_hand.get_value() > 21 :
                outcome = "you have busted"
                score -= 1
                score1 -= bet
                in_play = False
        else :
            outcome = "you have busted"
            score -= 1
            score1 -= bet
            in_play = False
            
def stand():
    global outcome, in_play, deck, pl_hand,de_hand,score,score1,bet      
    if in_play:
        if pl_hand.get_value()<=21:
            while de_hand.get_value()<17:
                de_hand.add_card(deck.deal_card())
            if de_hand.get_value()>21:
                outcome = "you win, dealer busted!"
                score += 1
                score1 += bet
                in_play = False
            elif de_hand.get_value() >= pl_hand.get_value():
                outcome = "dealer wins!"
                score -= 1
                score1 -= bet
                in_play=False
            else :
                outcome = "you win!"
                score += 1
                score1 += bet
                in_play = False
        else :
            outcome = "you have busted!"
            score -=1
            score1 -= bet
            in_play = False

def reset():
    global score,score1,bet
    score = 0
    score1 = 0
    bet = 0
    if not in_play:
        deal()
        
def bet1(xx):
    global bet 
    bet = int(xx)
    
# draw handler    
def draw(canvas):
    global outcome, in_play, deck, pl_hand,de_hand,score,score1,bet      
    if in_play:
        canvas.draw_text("Hit or stand?" ,[330,340],30,"orange",'sans-serif')
        outcome = ""
    else :
        canvas.draw_text("New deal?" ,[330,340],30,"orange",'sans-serif')    
    canvas.draw_text(outcome ,[170,170],30,"Yellow",'sans-serif')
    canvas.draw_text("New bet?" ,[330,370],30,"orange",'sans-serif')
    canvas.draw_text("Wins: " + str(score) ,[430,50],25,"Yellow",'sans-serif')
    canvas.draw_text("Score: " + str(score1) ,[430,90],25,"Yellow",'sans-serif')
    canvas.draw_text("Your bet: " + str(bet) ,[430,130],25,"Yellow",'sans-serif')
    canvas.draw_text("Blackjack" ,[60,90],40,"orange",'sans-serif')
    canvas.draw_text("Player" ,[60,170],30,"white",'sans-serif')
    canvas.draw_text("Dealer" ,[60,370],30,"white",'sans-serif')
    de_hand.draw(canvas,[60,400])
    pl_hand.draw(canvas,[60,200])

    if in_play:
        canvas.draw_image(card_back, [36,48], CARD_SIZE, [176,448], CARD_SIZE)
        
# initialization frame
frame = simplegui.create_frame("Blackjack", 600, 600)
frame.set_canvas_background("Green")

#create buttons and canvas callback
frame.add_input('Insert your bet', bet1, 200)
frame.add_button("Deal", deal, 200)
frame.add_button("Hit",  hit, 200)
frame.add_button("Stand", stand, 200)
frame.add_button("Reset score", reset, 200)
frame.set_draw_handler(draw)


# get things rolling
deal()
frame.start()
