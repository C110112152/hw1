library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL; 
use IEEE.STD_LOGIC_UNSIGNED.ALL; 

entity two_counter is
    Port (i_clk : in STD_LOGIC;
          i_rst : in STD_LOGIC;
          o_count1 : out STD_LOGIC_VECTOR (3 downto 0);
          o_count2 : out STD_LOGIC_VECTOR (3 downto 0)
         );
end two_counter;

architecture Behavioral of two_counter is
signal count1 : STD_LOGIC_VECTOR (3 downto 0);
signal count2 : STD_LOGIC_VECTOR (3 downto 0);
signal state : STD_LOGIC;
begin
    o_count1 <= count1;
    o_count2 <= count2;

    FSM : process(i_clk, i_rst)
    begin
        if i_rst = '0' then
            state <= '0';
        elsif i_clk'event and i_clk = '1' then
            case state is
                when '0' =>
                    if count1 = "1111" then
                        state <= '1';
                    end if;
                when '1' =>
                    if count2 = "0000" then
                        state <= '0';
                    end if;
                when others =>
                    null;
            end case;
        end if;
    end process;

    counter1p : process(i_clk, i_rst, state)
    begin
        if i_rst = '0' then
            count1 <= "0000";
        elsif i_clk'event and i_clk = '1' then
            case state is
                when '0' =>
                    count1 <= count1 + '1';
                when '1' =>
                    null;
                when others =>
                    null;
            end case;
        end if;
    end process;

    counter2p : process(i_clk, i_rst, state)
    begin
        if i_rst = '0' then
            count2 <= "1111";
        elsif i_clk'event and i_clk = '1' then
            case state is
                when '0' =>
                    null;
                when '1' =>
                    count2 <= count2 - '1';
                when others =>
                    null;
            end case;
        end if;
    end process;
end Behavioral;
