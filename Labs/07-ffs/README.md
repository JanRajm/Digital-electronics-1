 ## Cv. 7
 
 ### Preparation task
 | **D** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | Remember |
   | 0 | 1 | 0 | Remember |
   | 1 | 0 | 1 | Sample D |
   | 1 | 1 | 1 | Sample D |

   | **J** | **K** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | 0 | No change |
   | 0 | 0 | 1 | 1 | No change |
   | 0 | 1 | 0 | 0 | Reset |
   | 0 | 1 | 1 | 0 | Reset |
   | 1 | 0 | 0 | 1 | Set |
   | 1 | 0 | 1 | 1 | Set |
   | 1 | 1 | 0 | 1 | Toggle |
   | 1 | 1 | 1 | 0 | Toggle |

   | **T** | **Qn** | **Q(n+1)** | **Comments** |
   | :-: | :-: | :-: | :-- |
   | 0 | 0 | 0 | No change |
   | 0 | 1 | 1 | No change |
   | 1 | 0 | 1 | Invert |
   | 1 | 1 | 0 | Invert |

### 2)

### 3)

##### p_d_ff_rst
p_d_ff_rst : process (rst, clk)
    begin
               if (rising_edge(clk)) then
                   if rst = '1' then
                       q     <= '0';
                       q_bar <= '1';
                   elsif (rising_edge(clk)) then
                       q <= d;
                       q_bar <= not d;
                   end if;
               end if;
    end process p_d_ff_rst;
