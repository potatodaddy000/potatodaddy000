import discord
from discord.ext import commands

# Create an instance of the bot
bot = commands.Bot(command_prefix='!')

# Add roles
@bot.event
async def on_raw_reaction_add(payload):
    # Check if the reaction is on the ✅ emoji
    if payload.emoji.name == '✅':
        # Get the connected guild and channel
        guild = bot.get_guild(payload.guild_id)
        channel = guild.get_channel(payload.channel_id)
        # Get the user who reacted
        user = await guild.fetch_member(payload.user_id)
        # Get the "member" role
        role = discord.utils.get(guild.roles, name='member')
        # Check if the user already has the role or not
        if role not in user.roles:
            # If not, add the role to the user
            await user.add_roles(role)
            await channel.send(f'{user.mention} successfully added to the "member" role!')
        else:
            await channel.send(f'{user.mention} is already in the "member" role.')

# Command to send a welcome message and add automatic reaction
@bot.command()
async def verify(ctx):
    # Send welcome message
    await ctx.send('Hello! Welcome to Potato Shop Server. To see the rooms, click on the ✅ reaction.')
    # Add automatic reaction
    await ctx.message.add_reaction('✅')

# Run the bot with your token
bot.run('YOUR_TOKEN')
