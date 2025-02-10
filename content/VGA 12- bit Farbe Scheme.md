display a thin line (olive) across the screen.
```VHDL
library IEEE;
use IEEE.STD_LOGIC_1164.ALL;
use IEEE.STD_LOGIC_ARITH.ALL;
use IEEE.STD_LOGIC_UNSIGNED.ALL;

entity VGA_Border is
    Port (
        clk       : in  STD_LOGIC; -- Clock signal
        h_count   : in  INTEGER range 0 to 799; -- Horizontal counter
        v_count   : in  INTEGER range 0 to 524; -- Vertical counter
        video_on  : in  STD_LOGIC; -- Active video region
        pixel_out : out STD_LOGIC_VECTOR(11 downto 0) -- 12-bit color output
    );
end VGA_Border;

architecture Behavioral of VGA_Border is
begin
    process(clk)
    begin
        if rising_edge(clk) then
            if video_on = '1' then
                if (h_count = 0 or h_count = 639 or v_count = 0 or v_count = 479) then
                    pixel_out <= "100010000000"; -- Olive (R=8, G=8, B=0)
                else
                    pixel_out <= "000000000000"; -- Black background
                end if;
            else
                pixel_out <= "000000000000"; -- Outside visible area
            end if;
        end if;
    end process;
end Behavioral;
```

Try show a 140\*340 thin-lined Rectangle in the middle of the screen, with shades behind.