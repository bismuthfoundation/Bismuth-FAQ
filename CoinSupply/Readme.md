The following Python code calculates the total Bismuth coin supply when the block_height > 1450000.  
There was a hard fork which changed the mining rewards at block_height=1450000, and that explains why the total coin supply functions are written like this.  

Note also the SQL code after the comment symbol #: It calculates all mining rewards R0 from the genesis up to the hard fork at block_height=1450000.  

- The variable R1 contains the sum of all hypernode reward before block height 1450000.  
- The constant delta1=91e-8 describes how much the mining rewards decrease for every new block, while delta=33e-8 desribes how much the pos (hypernode) rewards decrease for each block.  
- The constant pow=5.5 describes the mining reward at block_height=1450000 and the variable N is the number of new blocks since the hardfork.  
- The constant dev_rew takes into account the 10% developer rewards which are calculated based on the mining rewards, and currently not on the hypernodes rewards.

~~~
def coin_supply(block):
    # select sum(reward) from transactions where block_height>0 and block_height<=1450000;
    R0 = 19061426.9635592
    # select sum(amount) from transactions where recipient='3e08b5538a4509d9daa99e01ca5912cda3e98a7f79ca01248c2bde16' and block_height>=-1450000;
    R1 = 920008
    delta1 = 91e-8
    delta2 = 33e-8
    pos = 2.4
    pow = 5.5
    N = block - 1450000
    dev_rew = 1.1
    R = dev_rew*R0 + R1 + N*((pos - N/2*delta2) + dev_rew*(pow - N/2*delta1))
    return R;

x = 1500000
print("Coin Supply at block {} = {}".format(x,coin_supply(x)))
~~~

Another approximate function of the Bismuth coin supply based on timestamps instead of the Bismuth block_height is shown below:

~~~
from datetime import datetime

def coin_supply(t):
    R0 = 19061426.9635592
    R1 = 920008
    t0 = datetime(2019,11,27,21,16,2).timestamp() # Hardfork UTC time
    delta1 = 91e-8
    delta2 = 33e-8
    pos = 2.4
    pow = 5.5
    N = (t - t0) / 60 # Estimated number of block since HF
    dev_rew = 1.1
    R = dev_rew*R0 + R1 + N*((pos - N/2*delta2) + dev_rew*(pow - N/2*delta1))
    return R;

x = datetime.now().timestamp() # Assuming system returns UTC time
print("Coin Supply at timestamp {} = {}".format(x,coin_supply(x)))
~~~
